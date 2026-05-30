---
title: "Hytrust Configuration Backups with Powershell REST API Calls"
date: 2017-08-02
media_subpath: /assets/images
permalink: /2017/08/02/hytrust-configuration-backups-with-powershell-rest-api-calls/
categories: 
  - "Disaster Recovery"
  - "VMware"
tags: 
  - "API"
  - "Encryption"
  - "HyTrust"
  - "HyTrust KeyControl"
  - "PowerShell"
  - "Rapid Recovery"
  - "REST API"
  - "Scripting"
  - "VM Encryption"
  - "VMware"
  - "vSphere 6.5"
  - "VMware vSphere"
image: "HytrustLogo.png"
---

![](Featured_KeyControlFlow.png)


What do you do when one of your most critical applications isn't supported by your backup vendor?  Do you find a new backup vendor, change applications, figure out a different solution, or cry silently in the fetal position on the server room floor? This, like any other problem, needed a solution, and this post is to share that solution with others.  Keep reading.

### The Problem

We had just deployed a 2-node Hytrust KeyControl v4.0-11538 cluster for use with VM Encryption (vCenter 6.5d, all ESXi hosts at 6.5d).  Each cluster node was running as a VM with one node inside a vSphere cluster and one node outside.  Everything worked great...except for backups using Rapid Recovery.  Since Hytrust runs a very customized version of FreeBSD 32-bit, we were not able to install the Rapid Recovery agent on either node.  Even if we could have installed the agent, Rapid Recovery does not support FreeBSD as an operating system on which their agent will run.  I had hoped we would still be able to backup at least one node agentlessly via the vSphere APIs for Data Protection, but Rapid Recovery does not support agentless backups of operating systems not supported by the agent per [this article](https://support.quest.com/technical-documents/rapid-recovery/6.1.2/release-notes/6#TOPIC-678584).

At this point, 3rd party backup with Rapid Recovery was ruled out completely.  And maybe that wasn't a bad thing.  That's what I kept telling myself anyway.

### Defining a Hytrust Cluster Failure

As mentioned above, we're using Hytrust KeyControl as a KMS for VM Encryption.  [This article](https://www.starwindsoftware.com/blog/encryption-of-vmware-vsphere-6-5-virtual-machines-and-vmotion-migrations-and-their-performance) explains more about the inner workings of VM Encryption from the standpoint of how vCenter and the KMS interact.

Hytrust KeyControl clusters provide active-active KMS functionality with the configuration fully synchronized between all cluster nodes.  In a 2-node cluster, if one node fails and cannot be recovered, you can easily remove the failed node from the cluster, spin up a new VM using the Hytrust OVA or ISO, and add it as a new member to the cluster.  That will fully mirror the configuration to the new node and bring back your redundancy.

What we were trying to guard against was the scenario of total KMS cluster failure.  If somehow both nodes of the cluster were to fail, it creates a very large problem.  If the encryption keys the cluster was managing were not backed up, there's no way to restore the KMS cluster and no way to decrypt the data.  That would mean the encryption keys for almost every VM in our datacenter just disappeared into oblivion.  For VM Encryption to work properly, you need both vCenter and a KMS online.

### Taking a Different Approach

I noticed there was an option within the Hytrust web GUI to take and download a backup of the cluster, but there was no option to do this on a schedule and download the backup for safe keeping.  In fact, it appears each time you perform a backup, it will overwrite the one saved on the appliance.  Downloading the backup is a manual process as shown here.

![](1_KeyControl_BackupPage.png)

There had to be a way to script this.  As luck would have it, the Hytrust appliance has REST APIs that can be leveraged for various tasks.  So I put on my Powershell hat and decided to go for it.  All of the Powershell commands shown here were done from a Windows 8.1 Pro machine running Powershell 4.0.

The first thing to do when connecting to a system like this is to login.  The $headers variable below needed to be added to login successfully via the API call with a Hytrust username and password.  My understanding is a user with secroot access is needed for all steps shown below to succeed.

```powershell
#Specify the fqdn of the server that you want backed up (could be either cluster node) 
$server="fqdn of my server" 

#Build the headers #The username and password parameters are specified in the programmer reference for Hytrust 
$headers=@{} $headers.add("username","myuser") $headers.add("password","mypassword") 

#Invoke the proper method to login and capture the authentication token as a variable (must be used to authenticate later API calls) 
$Token = Invoke-Restmethod -method POST -Uri "https://$server/v4/kc/login/" -body $headers`
```

Based on what you see here, if you were to run

```powershell
Invoke-Restmethod -method POST -Uri "https://$server/v4/kc/login/" -body $headers`
```

from a Powershell window, you will be met with the following error:

![](2_Invoke-RestMethod_Error_SSL_TLS.png)

Far searchability, here's error in full text:

> _Invoke-RestMethod: The underlying connection was closed: Could not establish a trust relationship for the SSL / TLS secure channel.  At line: 1 char: 1_

### The First Hurdle

Right out of the gate, we had an error trying to login to a Hytrust node using an API call.  To establish the trust mentioned above, I needed to add Hytrust to the Trusted Root Certification Authority store on my computer.  Use the steps below to get and install the proper certificate on your computer to clear this error:

- Login to one node of the Hytrust cluster (should not matter which one).

- After logging in with a user that has permissions to manipulate the cluster's KMIP configuration, click on the KMIP menu.  
![](3_SettingsEnable.png)

- Now make sure KMIP functionality is enabled for your cluster (needed when using VM Encryption), and apply the changes.  
![](4_KMIP_Enable.png)

- After the changes have been applied, click on Users.  This will take you to KMIP users.  
![](5_KMIPUsersOption.png)

- If you had to enable KMIP above, you probably don't have any KMIP users created.  If you're already using VM Encryption, you should have a user created in this area and can go straight to the step for downloading certificates.  To create a new user, go to Actions -> Create User.  
![](6_KMIP_CreateUser1.png)

- Give the user a name (whatever you want), and click Create.  There is no need to enter a password.  Notice the certs for this user will expire after 1 year by default (which could be changed if you want).  
![](7_KMIP_CreatUser2.png)

- Now that you see the newly created user on the list of KMIP users, click to select the user.  Then, go to Actions -> Download Ceritificate.  
![](8_KMIP_DownloadUserCertificate.png)

- This process should download a ZIP file to your computer containing two certificates - the CA certificate for Hytrust and a user-specific certificate for KMIP communication with another server (such as vCenter).  
![](9_CACert.png)

- Now, import the cacert.pem file into Trusted Root Certificate Authorities store on the computer you are using per [this article](https://technet.microsoft.com/en-us/library/cc754841\(v=ws.11\).aspx#BKMK_addlocal).

Once the above steps have been completed, running the command

```powershell
$Token = Invoke-Restmethod -method POST -Uri "https://$server/v4/kc/login/" -body $headers`
```

will place the proper authentication token into our $Token variable for use with future API calls.  If we were to show the contents of the $Token variable, it would look something like the screenshot you see below.  The result comes to us in the form of an array.  The access\_token field is unique to this specific login session (which will stay active for 1 hour by default unless you renew the token or logoff).

![](11_TokenContents.png)

### The Second Hurdle

At this point we were ready to make the magic happen and run a backup.  After running the following commands 

```powershell
#Build iDictionary object for calls to system_backup method 
$Params =@{} 
$Params.add("verify","false") 
Invoke-Restmethod -method POST -Uri "https://$server/v4/system_backup/" -headers $Token -body $Params
```

this error was generated:

![](10_CannotBindParameter.png)

Again, for searchability, we have:

> Invoke-RestMethod : Cannot bind parameter 'Headers'.  Cannot convert the ... value of type "System.Management.Automation.PSCustomObject" to type "System.Collections.IDictionary".

This specific issue was caused by the fact that $Token is a PSCustomObject variable, but we needed a variable of type iDictionary to use instead.  That is basically another array object in which we'll store the access\_token string from the array stored in the $Token variable.  Here's the bit of code I added to get around the problem and add the proper values into the variable $Token2:

```powershell
#Build new iDictionary object for the headers to future API calls 
$Token2=@{} $Token2.add("Auth-Token",$Token.access_token)`
```

I must confess I only knew to add the specific "Auth-Token" string as part of the iDictionary variable above after finding additional details on how the access token returned by the login call is used.  
![](12_AccessTokenDetails.png)

Our newly built command using $Token2 here will generate a success message.  It creates a .bu file you can download from the cluster node to which you are connected.

```powershell
Invoke-Restmethod -method POST -Uri "https://$server/v4/system_backup/" -headers $Token2 -body $Params`
```

Here's the success message you would see from the Powershell console:  

![](13_Result_Success.png)

If you have a SMTP server configured for your cluster, you should get an e-mail that a backup was created.  But now we need to download that backup.  If you take the command above and change the method to GET instead of POST, you will see what looks like The Matrix in the Powershell console window.  
![](14_TheMatrix.png)

Make sure to add the -OutFile parameter like this:

```powershell
#Download the backup to a file (needs to have a .bu extension) in the location of your choice 
Invoke-Restmethod -method GET -Uri "https://$server/v4/system_backup/" -headers $Token2 -body $Params -OutFile $FileName
```

This should generate another success message in the Powershell console.  The variable $FileName contains the full path to the backup file (something like C:\\MyBackup.bu for example), and you should get a success message from running the command assuming you have proper rights to the backup location specified.  Since the access token is good for one hour, go ahead and run the command below to logout:

```powershell
#Logout so the token is no longer valid Invoke-Restmethod -method POST -Uri "https://$server/v4/kc/logout/" -headers $Token2`
```

### Finishing Touches and Risk Mitigation

First, how do you know this backup file can be restored?  I spun up a new Hytrust cluster node and tested it to verify everything was indeed restored.  That would actually make for a great next blog post.

At this point, we had a way to take a backup.  We just needed somewhere to run the script on a schedule, somewhere to store the backups, and to set a retention period.  This is risky because based on the way I wrote the script the username and password must be stored inside the script.  I guess I could have tried to parameterize the script so as to pass those parameters within a scheduled task, but I didn't go there and am not certain it makes things more secure.  To mitigate the risk, here's what was done:

- The script is stored on a volume on a Windows Server that only the IT Department can access in any way (physical access restricted, NTFS sharing and security permissions restricted).
- The backups are stored on this same volume (also restricted).
- The volume mentioned here is backed up daily using our backup software and will be replicated offsite.
- The script runs daily via Windows Task Scheduler using a specific user account via the method described [here](https://community.spiceworks.com/how_to/17736-run-powershell-scripts-from-task-scheduler), which will bypass the server's Powershell execution policy only when the script tuns.
- The retention period for backups is 7 days per Hytrust's recommendation.
- E-mail alerts will fire any time a backup is taken.

### Resources

Since using APIs was completely new to me, here are some links I used to get started:

- Very helpful in getting started and building objects to pass parameters to API calls - [https://www.datacore.com/RESTSupport-Webhelp/using\_windows\_powershell\_as\_a\_rest\_client.htm](https://www.datacore.com/RESTSupport-Webhelp/using_windows_powershell_as_a_rest_client.htm)
- Great article on the difference between Invoke-RestMethod and Invoke-WebMethod - [http://wahlnetwork.com/2015/10/29/tackling-basic-restful-authentication-with-powershell/](http://wahlnetwork.com/2015/10/29/tackling-basic-restful-authentication-with-powershell/)
- For simple download / output of the backup to a file - [https://blog.jourdant.me/post/3-ways-to-download-files-with-powershell](https://blog.jourdant.me/post/3-ways-to-download-files-with-powershell)
- [Don't confuse VM Encryption with vSAN Encryption](http://www.yellow-bricks.com/2016/11/07/the-difference-between-vm-encryption-in-vsphere-6-5-and-vsan-encryption/)

Here's the full sanitized script that will retain all backups created in the last 7 days in a directory of your choice.  I probably could have put it on GitHub but did not go there this time.

```powershell
#Powershell Script to login to Hytrust VM and take a backup
#
#This script assumes you have the cacert for your Hytrust VM installed as a trusted root CA on the machine that is running Powershell
#
#Created by Nick Korte on 7/27/2017
#
#Version History
# v1.00 - Build
#
#Remove old backup files (anything older than 7 days)
$BackupDir = "Insert File Location Here"
$DaysOld = "-7"
$CurrentDate = Get-Date
$DeleteDate = $CurrentDate.AddDays($DaysOld)
Get-ChildItem $BackupDir | Where-Object { $_.LastWriteTime -lt $DeleteDate } | Remove-Item
#
#
#Specify the fqdn of the server that you want backed up
$server="fqdn of my server"
#
#Short server name for backup file name generation
$shortserver="server netbios name"
#
#
#Build the headers
#The username and password parameters are specified in the programmer reference for Hytrust
$headers=@{}
$headers.add("username","myuser")
$headers.add("password","mypassword")
#
#Invoke the proper method to login and capture the authentication token as a variable (must be used to authenticate later API calls)
$Token = Invoke-Restmethod -method POST -Uri "https://$server/v4/kc/login/" -body $headers
#
#Build new iDictionary object for the headers to future API calls
$Token2=@{}
$Token2.add("Auth-Token",$Token.access_token)
#
#Build iDictionary object for calls to system_backup method
$Params =@{}
$Params.add("verify","false")
#
#Take a system backup - this only creates the backup and does not download it
Invoke-Restmethod -method POST -Uri "https://$server/v4/system_backup/" -headers $Token2 -body $Params
#
#Construct date for file name yyyymmdd
$MyDate1 = get-date -uformat %Y
$MyDate2 = get-date -uformat %m
$MyDate3 = get-date -uformat %d
$MyDate = $MyDate1 + $MyDate2 + $MyDate3
#
#Construct time for file name hhmmss
$MyTime1 = get-date -uformat %H
$MyTime2 = get-date -uformat %M
$MyTime3 = get-date -uformat %S
$MyTime = $MyTime1 + $MyTime2 + $MyTime3
#
#File name will be yyyymmdd.hhmmss_servername.bu
$FileName = $BackupDir + "\" + $MyDate + "." + $MyTime + "_" + $shortserver + ".bu"
#
#Download the backup to a file (needs to have a .bu extension) in the location of your choice
Invoke-Restmethod -method GET -Uri "https://$server/v4/system_backup/" -headers $Token2 -body $Params -OutFile $FileName
#
#Logout so the token is no longer valid
Invoke-Restmethod -method POST -Uri "https://$server/v4/kc/logout/" -headers $Token2
```
