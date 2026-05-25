---
title: "Configuring a Dell PERC H730 Mini - Journey to vSAN 6.6"
date: 2017-04-26
media_subpath: /assets/images
permalink: /2017/04/26/configuring-a-dell-perc-h730-mini-journey-to-vsan-6-6/
categories: 
  - "Dell"
  - "VMware"
tags: 
  - "Dell"
  - "Dell PERC"
  - "JourneytovSAN"
  - "RAID"
  - "VMware"
  - "VMware vSAN"
  - "VMware vSAN 6.6"
  - "vSAN"
  - "vSAN 6.6"
image: "2_RAIDBIOSOption.png"
---

Originally posted on [MangoLassi](https://mangolassi.it/topic/13512/configuring-a-dell-perc-h730-mini-journey-to-vsan-6-6) April 26, 2017

Imagine having just racked 4 new Dell PowerEdge R730 servers. The network and power cables are all in place, and the disks are in the proper slots. The journey to deploy a new hybrid [vSAN 6.6](https://storagehub.vmware.com/#!/vmware-vsan/what-s-new-in-vmware-vsan-6-6) cluster has begun.

**Prerequisites** Before going any further, it is important to note that ESXi 6.0 was factory installed on mirrored SD cards within these hosts. Each host had to be upgraded to [ESXi 6.5d](http://pubs.vmware.com/Release_Notes/en/vsphere/65/vsphere-vcenter-server-650d-release-notes.html) to get the bits for [vSAN 6.6](https://storagehub.vmware.com/#!/vmware-vsan/what-s-new-in-vmware-vsan-6-6). That process will not be detailed here.

All host hardware is on the [VMware HCL for vSphere 6.5](https://www.vmware.com/resources/compatibility/search.php). The [PERC H730 Mini](http://www.vmware.com/resources/compatibility/detail.php?deviceCategory=vsanio&productid=34859&deviceCategory=vsanio&details=1&vsan_type=vsanio&io_partner=23&io_releases=278&keyword=PErC%20H730%20Mini&page=1&display_interval=10&sortColumn=Partner&sortOrder=Asc) as well as all capacity and solid state drives are on the [vSAN HCL for vSAN 6.6](http://www.vmware.com/resources/compatibility/search.php?deviceCategory=vsan).

**Jumping the Gun** After ensuring the hosts were at the proper ESXi patch, I couldn't wait to get my hands on the [vCenter Server Appliance 6.5d installer](https://my.vmware.com/web/vmware/details?downloadGroup=VC650D&productId=614&rPId=15962). After all, this version contains the [Easy Install](https://storagehub.vmware.com/#!/vmware-vsan/what-s-new-in-vmware-vsan-6-6/easy-install/1) workflow to make greenfield vSAN deployments simple. It will configure a vSAN datastore on one host of your choice and deploy a Platform Services Controller (PSC) and vCenter Server Appliance (VCSA) on that datastore. Once vCenter is up and running, you can use it to configure the rest of the hosts which will be part of the cluster.

I launched the installer from my workstation, and as I was stepping through it to install an external PSC, I reached the point for claiming disks in the target host for the initial vSAN datastore configuration. ![](1_EasyInstall.png)

As you can see, the installer can see no disks in the host. Oops.

**Back to Basics** I realized at this point that although all hardware was on the proper HCLs, I had forgotten to check the configuration of the PERC card in my target host. I connected to our KVM switch and rebooted the host. And not long after that, the problem was very obvious. Even though there were no virtual drives setup, all physical disks were marked for RAID use. ![](2_RAIDBIOSOption.png)

After entering the configuration utility for the PERC card, here's what I found: ![](3_PercVDMgmt.png)

All disks show to be unconfigured. That only means the physical disks were unconfigured for any kind of RAID level. They were still, however, marked as usable in a RAID configuration. If it is not highlighted already, use the up arrow to highlight the PERC card, and press F2 to get to the Operations menu from here. You won't see these options if any specific disk is selected.

![](4_ConvertToNonRAID.png)

Once you select Convert to Non-RAID and press Enter, you are immediately taken to the screen below where you must select which physical disks will be converted to Non-RAID disks. Use the space bar to mark each disk as Non-RAID. In my case, there were 12 physical disks. Once all disks have been marked, use the right arrow to highlight the OK button, and then hit Enter.

![](5_SelectDiskstoConvert.png)

At this point, here is what you see inside the Virtual Disk Management menu. ![](6_NoConfigPresent.png)

Press ESC to exit the configuration utility, and the host will reboot. This time when the host rebooted, it's easy to see that we have 12 disks ready for vSAN. ![](7_PercPost.png)

And this time, when I ran through the Easy Install, you can see there were disks ready to be claimed for use with vSAN as expected. ![](8_EasyInstallAgain.png)

**Lessons Learned** In the end, it was pretty simple to configure the PERC card in each server for use with vSAN. Don't make the mistake of assuming the vendor has configured the hardware optimally for the technology you are implementing.
