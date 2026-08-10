---
title: "Patching vCenter with External Platform Services Controller to Version 6.5.0d - Journey to vSAN 6.6"
date: 2017-04-17
media_subpath: /assets/images
permalink: /2017/04/17/patching-vcenter-with-external-platform-services-controller-to-version-6-5-0d-journey-to-vsan-6-6/
categories: 
  - "VMware"
tags: 
  - "JourneytovSAN"
  - "vCenter"
  - "vCenter 6.5"
  - "vCenter 6.5d"
  - "vCenter Patching"
  - "vCenter Update"
  - "vCenter with External PSC"
  - "VMware"
  - "VMware vCenter"
  - "VMware vSAN"
  - "vSAN"
  - "vSAN 6.6"
---

*Originally posted on MangoLassi April 17, 2017*
<!-- dead link was https://mangolassi.it/topic/13357/patching-vcenter-with-external-platform-services-controller-to-version-6-5-0d-journey-to-vsan-6-6 -->

Just yesterday, VMware released a [patch to vCenter](http://pubs.vmware.com/Release_Notes/en/vsphere/65/vsphere-esxi-650d-release-notes.html?src=vmw_so_vex_akjaer_1025) that enables functionality supporting vSAN 6.6. This patch is build 5310538 and version 6.5.0d of the vCenter Server Appliance. If you are chomping at the bit to play with [all the new features of vSAN 6.6](https://www.virtual-allan.com/vsan-6-6-esxi-6-5d-vcenter-6-5d-and-more-released/), applying this update is a must.

In the spirit of this update's release, I thought I would show how to patch an existing vCenter environment. In this example, we have the vCenter Server Appliance (VCSA) with an external Platform Services Controller (PSC). The reason for implementing in this way is to utilize Enhanced Linked Mode with future vCenter instances at other sites.

#### Before You Start, Check Backups
Before installing any patches, it is wise to have a good backup of all PSCs and the VCSA. One option is to take a [configuration backup of the PSCs and the VCSA](https://www.vladan.fr/vmware-vcsa-6-5-backup-and-restore-how-to/), but an image-level backup would also work. There is also the option to take a snapshot of each of these VMs before applying the patches, and then remove the snapshots once all functionality is verified.

#### Assumptions 
The steps here assume all of your PSCs and VCSA can connect to the internet to download updates from a VMware repository. Other methods for patching vCenter such as using the ISO will not be completely covered here but follow a similar method. This article also assumes the VCSA is not using vCenter HA.

#### Building Blocks
The version number of the PSC or VCSA can be checked using the VAMI (VMware Appliance Management Interface). This can be accessed via port 5480. So, we would visit **https://mypscorvcsaaddress:5480**, and login with the root credentials. Then click on the Update menu in the left-hand Navigator window. Notice from the screenshots here that the build number indicates we are running [vCenter 6.5.0a](http://pubs.vmware.com/Release_Notes/en/vsphere/65/vsphere-vcenter-server-650a-release-notes.html).  
![](1_extPSC.png)  
![](2_extVCSA.png)

#### Start with the PSCs
If you are using an external PSC with the VCSA, you may have more than one in your SSO domain. Make sure and repeat the steps here for all PSCs **before** updating the VCSA. Once inside the VAMI's Update menu, look in the area labeled Available Updates. In the case of this PSC, automatic checking for updates has been disabled.  
![](3_PSCUpdateAvailable.png)

Since this PSC has internet access, we can manually check for updates by clicking Check Updates and then clicking Check Repository.  
![](4_CheckRepository.png)

This will contact a VMware repository (address set by default upon install of the PSC or VCSA and shown in screenshot below) to check for any additional updates to vCenter.  
![](5_UpdateSettings.png)

After a check for updates, we can easily see the patch for 6.5.0d is available and that we pulled it down from the repository URL. If you were updating via an ISO, that would be indicated here as the update source. Click Install All Updates to continue.  
![](6_InstallAllUpdates.png)

At this point, you will have to accept the EULA to begin the install.  
![](7_EULA.png)

Once the install completes, you will likely be prompted to reboot as seen here.  
![](8_InstallUpdatesComplete.png)

After clicking OK, notice what the Update menu looks like. The updates have been installed but not applied.  
![](9_UpdatesInstallnotApplied.png)

To reboot, go to the Summary menu, and click Reboot.  
![](10_Reboot.png)

Once the PSC has rebooted, it will run a health check to ensure all is well. The Summary tab should look similar to this.  
![](11_PSCSummary.png)

Now, go back to the Update menu. Notice the new version has been installed and is running successfully. Checking for updates again shows we are on the latest update possible version of vCenter now.  
![](12_NoUpdatesAvailable.png)  
_Remember, this process must be followed for ALL PSCs in the SSO domain before proceeding to update the VCSA!_

#### And Now for the VCSA
Since the PSC had been patched successfully, I wanted to see if I could login to the VCSA since it was on a different patch than the PSC. I found that whether logging in as the SSO administrator for vCenter or a Windows user with vCenter access, I could login to vCenter successfully. I even tried rebooting the VCSA to see if that would make a difference, and even after a reboot, I could login to vCenter with no issues. It seems a minor patch level difference between PSC and VCSA may allow normal functionality, but certainly a major version difference (i.e. one on 6.0 and the other on 6.5) would prevent everything from working as expected (especially since the upgrade from 6.0 to 6.5 creates a new VCSA).

Similar to the process followed for the PSC, visit the VAMI for the VCSA, go to the Update menu, and check for updates. Install the update that is found, and reboot the VCSA once it completes.

At this point, make sure the proper version of vCenter shows to be running after the reboot of the VCSA as seen in the screenshot here.  
![](13_VCSASummary.png)

Then test to make sure login to the VCSA works (with users like the SSO administrator and Windows users if Active Directory has been setup as an identity source). Keep in mind it will take longer to be able to access the vSphere Client login page after the reboot than to login to the VAMI. Be patient, and wait for the web server to initialize.

When you reach the vSphere Client login page, you have the option to download the Enhanced Authentication Plugin. Even though this is a minor patch to vCenter, one may need to update the version of the Enhanced Authentication Plugin for SSO to work properly. Test, test test!  
![](14_EnhancedAuthPlugin.png)

#### What Does This Mean?
This means vCenter is now running at the latest patch level with vulnerabilities patched and bug fixes applied. We could have patched using an ISO or via the CLI, but using the VAMI is a very simple way to patch vCenter and all PSCs. Remember to remove any snapshots you may have created before the install!

For more information on patching the VCSA, check out the [vSphere Upgrade Guide](https://pubs.vmware.com/vsphere-65/topic/com.vmware.ICbase/PDF/vsphere-esxi-vcenter-server-65-upgrade-guide.pdf) starting on page 203.
