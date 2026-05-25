---
title: "Deploying the Mangle OVA on vSphere - A Story of Heartache and Triumph"
date: 2021-06-22
media_subpath: /assets/images
permalink: /2021/06/22/deploying-the-mangle-ova-on-vsphere-a-story-of-heartache-and-triumph/
categories: 
  - "VMware"
tags: 
  - "Chaos Engineering"
  - "Deployment"
  - "Embedded Host Client"
  - "ESXi"
  - "Mangle"
  - "Open Source"
  - "Open Source Software"
  - "OVA"
  - "OVA Deployment"
  - "vCenter"
  - "vExpert"
  - "Virtual Appliance"
  - "Virtual Machines"
  - "VMware Mangle"
  - "VMware Open Source"
  - "VMware vSphere"
  - "VMware vSphere 7"
  - "vSphere 7"
  - "vSphere Client"
image: "ruin-5337039_640.jpg"
---

I recently learned VMware has an open source project for chaos engineering called [Mangle](https://vmware.github.io/mangle/), and I thought it would be fun to do a little exploring.  After all, the name sounds pretty cool, and I know almost nothing about chaos engineering.  So let's create a little chaos, shall we?  This will be the first in a series of posts exploring VMware Mangle.

The [main Mangle site](https://vmware.github.io/mangle/) describes what Mangle does, where it can be deployed, and has documentation on the deployment and use of the product as well as [how you can contribute](https://vmware-1.gitbook.io/mangle/contributing-to-mangle).  The start of the official documentation can be found [here](https://vmware-1.gitbook.io/mangle/).

Being a former vSphere administrator, I figured the fastest path to stand up Mangle would be to deploy the OVA on vSphere.  Keep in mind that is the deployment option I'm choosing for this post and not the only option (could be deployed in public cloud somewhere if you like).  This post will detail my deployment process, including any errors I run into along the way.  I would encourage you to read the whole thing before deciding what steps to replicate for yourself.

 

### Standing up a Small Lab

First, I downloaded one of the latest versions of ESXi (7.0 U2a) as well as the accompanying version of the vCenter appliance.  The steps are not shown here (see [this doc page](https://docs.vmware.com/en/VMware-vSphere/6.7/com.vmware.esxi.install.doc/GUID-016E39C1-E8DB-486A-A235-55CAB242C351.html) for details on downloading ESXi), but you will need a [MyVMware](https://my.vmware.com/) (or Customer Connect as we call it now) account to download.  My laptop is fairly powerful, so I figured it would be easy to run nested ESXi on VMware Workstation.  As a first step, I checked the [Interoperability Matrix](https://interopmatrix.vmware.com/#/Interoperability?isHideGenSupported=true&isHideTechSupported=true&isHideCompatible=false&isHideIncompatible=false&isHideNTCompatible=false&isHideNotSupported=true&isCollection=false&col=455,5099,4175,3878,3418,3384,2745,2753,2590,2348&row=1,5087,4275,3495,3456,3221,2861,2735,3363,2731,2331,2131,2135,994,694,430,795,620,577,507,1032,796,559,441,253,1033,500,391,251,141,2980,18,243,96,17,16,15,14,181) and found this requires VMware Workstation 16 Pro.  I was fortunate to have a license to use it through the [vExpert program](https://vexpert.vmware.com/), but there is an option to do a 30-day trial (download [here](https://www.vmware.com/products/workstation-pro/workstation-pro-evaluation.html)).

There's something soothing about the console view of an ESXi host, isn't there?  Maybe it's just me.  In any case, I have a single ESXi 7.0 U2a host deployed using VMware Workstation (steps to get there not shown here).  It shows 2 CPU and 4 GB memory, which may or may not be  enough for what lies ahead, but I am leaving it for now.

![](1_hostconsole.png)

 

### Taking a Shortcut

At this point, I could go ahead and deploy vCenter on my virtual ESXi host, but I'm not going to do that.  This is a lab environment, and I want a quick win.  I'm going to see if I can deploy the Mangle OVA directly on this ESXi host.  I [had trouble doing this once with a Hytrust OVA]({% link _posts/2017-07-17-error-deploying-hytrust-ova-in-the-esxi-6-5-embedded-host-client-and-learning-to-read.md %}), but maybe things are different with the Mangle OVA and vSphere 7. Most would deploy an OVA through vCenter, but there is an option to deploy OVAs inside the ESXi embedded host client.  Why not try it?

Before we continue, I want to share some background information on my config.  The host is a VM that has ESXi installed on a 20 GB thin provisioned virtual disk (VMDK).  I've also added a second thin provisioned virtual disk (100 GB thin provisioned VMDK) to use as the host's local VMFS datastore.  The datastore will be where we store virtual disk files for any VMs running on the host (i.e. for the Mangle appliance).  Here's what the settings of my ESXi host VM look like at present.

![](2_VMSettings.png)

We saw the ip of the host above, so if I visit https://192.168.72.128/ui/#/login in a web browser on my laptop, I can login to the host itself using the ESXi embedded host client interface.  I'll login with the only username and password I have so far (username root and password I created during the ESXi install).

![](3_hostlogin-1024x983.png)

Once logged into the host, we can select the option to Create / Register VM.

![](4_createvm-1024x690.png)

 

[This Mangle documentation page](https://vmware-1.gitbook.io/mangle/mangle-administration/supported-deployment-models) contains the link to download the OVA for the latest version of Mangle and steps to deploy it.  I have the OVA downloaded and stored locally on my computer.  Follow along with the document mentioned if you like, but I'm going to step through it here.  Select the option below to Deploy a virtual machine from an OVF or OVA, and click next.

![](5_vmfromova-1024x647.png)

Give the new VM for Mangle a name (NN-VM-Mangle in this case), select the proper OVA to use (the file was downloaded earlier to the local drive of my laptop), and click Next.

![](6_MangleInstaller-1024x648.png)

As for selecting where the new VM will be stored, I have only one local datastore on this host named NN-ESXi-DS1, which is already selected.  Click Next to continue.

![](7_SelectStorage-1024x651.png)

At this point we have to accept the EULA.  Click the "I agree" button, and then click Next.  An error will pop up if we click Next before clicking "I agree," and once "I agree" is clicked, it becomes greyed out and is no longer clickable.

![](8_LicenseAccept-1024x646.png)

We will keep the default Deployment options (no changes to network mapping, thin provisioning, power on the VM automatically after deployment) and click Next to continue.

![](9_DeploymentOptions-1024x645.png)

Now we've reached some of the Mangle appliance configuration settings.  As shown below, we've already expanded the Application section and typed in an initial root password.  The information buttons on the far right explain the meaning of each field on this screen.  We'll go ahead and leave Enable SSH service in mangle appliance and Enable first boot for mangle selected.  Expand Networking Properties to see other information needed on this screen.

 

![](10_AdditionalSettings1-1024x656.png)

Ah, yes - Networking Properties.  We can enter all static ips and domain information here, but I'm choosing to leave all fields blank, which will automatically use DHCP.  Click Next to continue.

![](11_AdditionalSettings2-1024x644.png)

Just to prove it was not me guessing what would happen if fields were left blank, here's a shot of one of the information icons from the menu above telling us that if a field is left blank, that setting will default to DHCP.

![](12_LeaveBlank-1024x128.png)

We've reached that pinnacle screen right before a VM gets deployed.  All of our settings are there, including those left blank so we can use DHCP.  Click Finish to see what happens.

![](12_ReadytoComplete-1024x648.png)

Looking at the task pane for this host, we appear to be off to the races!

![](13_TaskPane1-1024x122.png)

After just a minute or two, it looks like disks got uploaded and the VApp imported, but there was some kind of error powering on the Mangle appliance.  Click the link at the top of the browser window to find out what happened.  After clicking the link, you can dismiss the alert.

![](14_PowerOnError-1024x523.png)

Now it makes sense.  The virtual appliance wants 4 vCPU.  Our host is saying it only has 2 CPU to provide.  As you can see in the screenshot above as well as that first screenshot with VM details from VMware Workstation earlier, we only allocated 2 vCPU to the virtual ESXi host.

![](15_ErrorDetails-1024x335.png)

When the ESXi VM was built, I never changed the default vCPU count away from the minimum requirements for an ESXi host.  The fix in this case was to power down the ESXi VM and increase the processor count.  The processor on my laptop is a dual 6 core processor, so I decided to give the ESXi VM 1 CPU socket with 6 cores.  Hopefully that will do the trick.

![](16_ReconfigureVM.png)

Once the ESXi host is powered back on again, we can confirm it sees 6 CPU to use.  The ESXi hosts believes these CPU are a single core each.  Our VM remains powered off because it did not power on automatically when the host did.  Click on the VM to look at the details of it.

![](17_6CPU-1024x395.png)

Notice the VM for Mangle shows to be allocated 4 vCPU and 8 GB memory.  Interestingly enough, not having enough CPU won't let us power on the VM, but we can overallocate RAM no problem.  Click Power on to see if this works.

![](18_PowerOnVM-1024x383.png)

As the VM starts to boot, we can see a preview of the console.  Click on the thumbnail below to open a console window to the Mangle appliance automatically (opens in a new browser window).  Alternatively, we could have clicked the thumbnail to power on the machine and open a console window automatically.

![](19_MangleConsolePreview-1024x357.png)

At first, things look promising.  It appears all first boot tasks are happening, and we are almost ready to be able to login to the Mangle appliance.

![](20_MangleVMConsole-1024x439.png)

After a couple of minutes, the console changed a little, and we've run into another issue.  The appliance did not detect any networking present.  Maybe this is due to the fact that I'm running a VM on top of a nested ESXi host?

![](21_NoNetworkDetected-1024x799.png)

Let's double check something back inside our ESXi embedded host client.  You can see that the virtual NIC for the appliance is connected and seems functional from the standpoint of what the host sees (connected to the only network that exists inside the standard virtual switch on the ESXi host).

![](22_VMNetworkConfig-1024x510.png)

What if we try logging into the appliance from the console as was recommended and run the command (/opt/vmware/share/vami/vami\_config\_net)?  We'll login with username root and the password set during the deployment of this VM. Running the above command gives a menu of options.  If we select option 0, we would expect to see nothing, right?  Let's check that.

![](23_ConsoleLogin1-1024x497.png)

There is only one setting - hostname.  Everything else is blank.  What now?

![](24_ConsoleLogin2-1024x801.png)

 

### Hitting a Wall

Sometimes shortcuts don't get you where you need to go quickly, and in this case it did not get me there at all.  I tried all of the following to no avail:

- Setting a static ipv4 address for the appliance
    - I could ping the ip address from my laptop but never could access https://ipaddress/mangle-services successfully.
- Setting the ipv4 address to DHCP
    - The appliance got an ip I could ping once again from my laptop, but I still could not access the web interface for the appliance.
- Rebooting the appliance after setting an ip
    - The same issue persisted as documented above.
- I tried re-deploying the appliance from scratch but had the same issue regardless of whether I left all Network Properties blank or set static ip addresses.
- I remembered you can deploy an OVA in VMware Workstation directly, but I ran into the same problem with this approach also.

When you hit a wall, you ask for help.  I found from someone on the Mangle team that at the time of this article's publishing, what I describe above is a known issue when deploying the OVA directly on an ESXi host (and is also a known issue deploying other Photon OS OVAs using this method).  The recommendation was to deploy the OVA using vCenter to avoid this issue.

So much for taking a shortcut.  The documentation didn't call out vCenter specifically but did hint at it based on screenshots.  Sometimes you need to do things the wrong way first to do them the right way the next time.

 

### Deploying the Mangle OVA Using vCenter

To this point I have deployed a vCenter running on top of the virtual ESXi host mentioned earlier (NN-ESXi1) after allocating more RAM to it.  The name of the vCenter is NN-vCenter1 with deployment size set to Tiny.  Our host NN-ESXi1 is under its management now inside a datacenter called NN-DC1.  Let's walk through the steps to deploy the Mangle OVA using vCenter.

Login to vCenter as the SSO administrator, and we should go straight to the hosts and clusters view.  Right-click the host, and select Deploy OVF template.

![](25_mangle3.x_step1_deployovftemplate-1024x400.png)

 

At this point, there are two options for grabbing the OVA file.  If vCenter can reach the internet, we can leverage a URL to download the OVA ([this doc](https://vmware-1.gitbook.io/mangle/mangle-administration/supported-deployment-models) has the URL for the latest version of Mangle).  Click Next to continue.

![](26_mangle3.x_step2a_selectovftemplate_url-1024x537.png)

If the OVA has already been downloaded (which was the case for me), just browse to the location on your computer where the OVA is stored, select it, and click Next.

![](26_mangle3.x_step2b_selectovftemplate-1024x536.png)

Give the new VM a name (in this case NN-Mangle1), and select the location for deployment.  In this lab, there is only one datacenter location with no folders created yet, so we select NN-DC1.

![](27_mangle3.x_step3_nameandfolder-1024x534.png)

As for compute resource, we only have one host to select, and there are no compatibility issues with the OVA being deployed to that host.  In a production environment our choice would likely be a specific cluster but could also be one specific host.  Click Next to continue.

![](28_mangle3.x_step4_computeresource-1024x529.png)

Now we review the template details and can confirm this is Mangle version 3.0.  Click Next to continue now that our list of deployment steps has expanded.

![](29_mangle3.x_step5_review-details-1024x530.png)

Check the box labeled "I accept all license agreements" followed by clicking Next.  Clicking Next before accepting the license agreement will throw an error and prevent continuation of the deployment steps.

![](30_mangle3.x_step6_accept_license-1024x533.png)

Now it's time to select the virtual disk format for the appliance VMDKs and the datastore in which they will reside.  Let's choose thin provisioning (only uses 2.7 GB per the above screenshot), and there is only one datastore in this environment (NN-ESXi1-DS1).  Click Next to continue.

![](30_mangle3.x_step7_select_storage-1024x530.png)

Select the virtual networks to use for the appliance.  Only a single virtual network exists in the environment, so we will use VM Network here (selected by default in my environment).  We will stick with IPv4 and click Next.

![](31_mangle3.x_step8_select_networks-1024x528.png)

Once we reach the Customize template screen, we are immediately alerted that 1 property has an invalid value.  What could that be?

![](32_mangle3.x_step9a_invalid_property-1024x530.png)

If you remember from trying this deployment directly on a host earlier, we have to set the appliance root password (the only required fields on this screen).  Once we set a password, the error message turns into a success message.  But don't click Next yet.  Scroll down to look at the Networking Properties section.

![](32_mangle3.x_step9b_pwd_entered-1024x537.png)

It's easy to see that if a value is left blank in this area, we are communicating that we want to use DHCP.  Just like before, we will leave that section blank to use DHCP and click Next to continue.

![](33_mangle3.x_step9c_network_properties-1024x540.png)

We've reached that final screen before starting the deployment.  Review the settings to double check everything, and click Finish to begin the deployment.

![](34_mangle3.x_step10a_ready-1024x536.png)

![](34_mangle3.x_step10b_ready-1024x535.png)

It looks like the deployment completed successfully, and we can see the new VM in vCenter's inventory.  Let's see what happens when we power it on.

![](35_mangle3.x_step11_poweron-1024x421.png)

Similar to before, we can open a console to keep an eye on things.  This boot process looks normal.

![](36_mangle3.x_step12_console1-1024x616.png)

Look at that.  We have an ip address on boot from DHCP.  Maybe, just maybe, this worked as expected.

![](37_mangle3.x_step12_console2-1024x757.png)

The console above appears to be the only place that allows setting the time zone for the appliance.  I recommend doing that before proceeding (steps not shown here).

Let's try visiting that web address in a browser.  It will be https://ipaddressofappliance/mangle-services.  This time the link worked to access the web interface of the appliance!  I did get the normal Google Chrome security warning about an insecure certificate but chose to proceed.  The first thing we are prompted to do (even before we have the option to login) is change the password for user admin@mangle.local.  This is going to be different from the root user whose password we set during appliance deployment.   Enter the password here and click UPDATE to continue.

![](38_mangle3.x_step13_changepwd-1024x355.png)

Now we're at the appliance login screen.  The only domain to select is the mangle.local domain.  We'll login as user admin and the password set in the previous step.  The domain has to be selected, and the username has to be just admin (not admin@mangle.local) to avoid throwing an error.

![](39_mangle3.x_step14_welcometomangle-1024x828.png)

And just like that, we are officially logged into the Mangle appliance.  It was a hard fought battle, but I consider it a success in the end.

![](40_mangle3.x_step15_loggedinnow-1024x565.png)

 

### Lessons Learned

That, dear readers, is where we stop for now.  As we discovered, there are some small differences in the workflow when deploying an OVA directly on an ESXi host compared to deploying that same OVA using vCenter.  In my opinion, the vCenter workflow was a bit cleaner, but both seemed very intuitive.

In this case taking a shortcut did not payoff, but now we have a newly deployed appliance with which we can start to tinker in future posts.  Where do we go from here?  As the story progresses, I will update with more blog posts.
