---
title: "Tales of a vCenter Upgrade Gone Wrong - Journey to vSAN 6.6"
date: 2017-05-05
media_subpath: /assets/images
permalink: /2017/05/05/tales-of-a-vcenter-upgrade-gone-wrong-journey-to-vsan-6-6/
categories: 
  - "VMware"
tags: 
  - "Journeytovsan"
  - "vCenter Upgrade"
  - "VMware"
  - "VMware vCenter"
  - "VMware vCenter Upgrade"
  - "VMware vSAN"
  - "VMware vSAN 6.6"
  - "VMware vSphere"
  - "VMware vSphere 6.5"
  - "vSAN"
  - "vSphere"
---

Originally posted on [MangoLassi](https://mangolassi.it/topic/13656/tales-of-a-vcenter-upgrade-gone-wrong-journey-to-vsan-6-6) May 5, 2017

All I did was upgrade vCenter 6.0U3 to 6.5a, and things went horribly wrong. Before getting into the details, let me take you back to where our story begins.

**Where We Began** It all started when we began setting up new hardware to completely gut and rebuild our infrastructure. We started at site 1 of 3.

We had two Dell PowerEdge R620 servers, some PowerConnect N4032 switches, and a MD3820i SAN with plenty of storage. The R620s came from the factory with ESXi 6.0U2 installed. We had license to both vSphere Standard for all hosts as well as vCenter Standard. The end goal was to put 1 vCenter instance at each site and use Enhanced Linked Mode between the different vCenter instances. For Enhanced Linked Mode to work as expected, we had to have one or more external Platform Services Controllers.

Since we had planned to have only two hosts inside the cluster at site 1 using the same back end storage, I used a different server outside the cluster running an evaluation version of ESXi 6.0U2 and local storage to get started with vCenter.

This was early February 2017, so there was no 6.0U3 yet (not released until 2/24/2017). Even though 6.5a had just been released on 2/2/2017, our backup software vendor was not yet officially supporting vSphere 6.5. Also, the vCenter plugins for management of the MD3820i were only supported in vCenter 6.0. At this point we could not yet upgrade to vSphere 6.5 but agreed that we would once all other environmental components were officially supported.

Since the hosts were at 6.0U2 and I was chomping at the bit to get started, I grabbed the vCenter Server 6.0U2a installer from VMware's site and went for it. I deployed the Platform Services Controller (PSC) appliance and a Windows vCenter Server (just in case we needed Update Manager) on the ESXi host outside the cluster. We used vCenter to configure our cluster without any trouble, and vCenter 6.0U2a was working like a champ.

**The Plot Thickens** As described in [this post](https://mangolassi.it/topic/12449/vendor-mistake-vmware-infrastructure-decisions), we were not able to use the SSDs we had purchased as cache for the SAN, so we began testing Infinio as a caching mechanism. At the time, Infinio recommended we update everything to vSphere 6.0U3. I went ahead and upgraded the PSC and vCenter to 6.0U3 and used Update Manager to update the hosts to 6.0U3. This was early March 2017.

At this point we had vCenter 6.0U3 working fine. As you can see from the linked post above, we ended up sending the R620s and the MD3820i back to Dell in exchange for 4 R730s that we would use as a vSAN cluster at site 1. We would eventually follow a similar pattern at other sites.

Since we already had a vCenter and PSC outside the cluster, I decided to upgrade them both to 6.5a while we were waiting on the new R730s to arrive after confirming this would be fully supported by our backup vendor. I used the vCenter Server Appliance installer and the built-in migration assistant to migrate the PSC to 6.5a and migrate the Windows vCenter instance to the vCenter Server Appliance 6.5a. 

I documented the entire process. The migration assistant worked like a champ to create a new PSC and VCSA running 6.5a with all of my configurations and history saved.

At this point we were at version 6.5a with vCenter and the PSC.

**When Things Went Haywire** The new R730s arrived in mid-April 2017, and each one had 6.0U3 factory installed. Around that same time, the newest version of vSAN was released as part of vSphere 6.5d, so the first thing I did was upgrade the PSC and VCSA to 6.5d. I have outlined how simple that process is to do in [this post]({% link _posts/2017-04-17-patching-vcenter-with-external-platform-services-controller-to-version-6-5-0d-journey-to-vsan-6-6.md %}).

We had a PSC and VCSA running the latest and greatest version (6.5d) on a host outside of the cluster we were about to build. I was able to use Update Manager to upgrade all of the new R730 servers to ESXi 6.5 (using the Dell ISO for ESXi 6.5) and then patch them (with a host patch baseline) to 6.5d.

Before adding the new R730 servers to vCenter and starting the vSAN build, I realized I still had an empty cluster object sitting in vCenter. To start fresh, I went ahead and deleted the cluster object from the vSphere Web Client, and that is when vCenter went completely nuts on me. After right-clicking the cluster and selecting the option to delete it, I would see it disappear from the object tree on the left but would also see a task in the task pane that would never complete.

![](1_RecentTasks.png) ![](1_RecentTasks.png)

Then, everything seemingly disappeared from the host and clusters view, the vms and templates view, and essentially everywhere else inside the vSphere Web Client.

Hosts and Clusters View ![](2_HostandClusters.png)![](2_HostandClusters.png)

VMs and Templates View ![](3_VMsandTemplates.png)![](3_VMsandTemplates.png)

Then, eventually, this error showed near the top of the vSphere Web Client.

![](4_CouldnotConnect.png) ![](4_CouldnotConnect.png)

Trying the vSphere Client instead showed a 503 error.

![](5_503Error.png) ![](5_503Error.png)

At this point, vCenter was essentially hosed. I rebooted it from the VAMI. After the reboot, everything was fine again. I tried deleting the cluster again only to have the same thing happen. The only way to bring vCenter back was to reboot the VCSA. This time I tried creating a new cluster object and then deleting it. The exact same thing happened. I tried rebooting the PSC first and then the VCSA, but that did nothing for me either. It seemed like I could do anything in vCenter except delete a cluster. It may seem small, but what else may have gone wrong if I had not found the problem before building the vSAN cluster?

I had no choice at this point but to contact VMware Support. The support technician from VMware showed me that the vxpd process would crash any time we deleted a cluster object in vCenter. The technician recommended I backup the vPostgres database and restore onto a VCSA with the same name and ip to see if that resolved the issue so as to retain all vCenter data.

**Lessons Learned** After getting some additional help on Twitter, I eventually found my upgrade from 6.0U3 to 6.5a back in March is not officially supported by VMware.

Since we had very little historical data in vCenter, I ended up deploying a fresh PSC and VCSA using the vCenter 6.5d installer and utilizing the [Easy Install for vSAN 6.6](https://storagehub.vmware.com/#!/vmware-vsan/easy-install). That allowed me to setup all of the host networking appropriately, claim disks on all hosts, and use the configuration assistant to provision the vSAN cluster with no issues.

Why wasn't my upgrade supported? It turns out vSphere 6.0U3 was released on 2/24/2017, which was actually after 6.5a was released (2/2/2017). And as is clearly stated in the [release notes for vCenter 6.0U3](https://pubs.vmware.com/Release_Notes/en/vsphere/60/vsphere-vcenter-server-60u3-release-notes.html#clientissues), you can see that an upgrade from here to vCenter 6.5 is not supported.

Always, always, always check to see if moving from your version to a higher version of the product before performing upgrades. In my case, I wish the installer for vCenter 6.5a had stopped me from upgrading altogether. Even though I goofed up the upgrade the first time, I learned the product so much better by having to re-create from scratch.

I'd also like to give a special thanks to Adam Eckerle of the VMware Technical Marketing Team for his help and advice in figuring out the root problem and helping me get it resolved.
