---
title: "Journey to vSAN - A Technical Adventure"
date: 2018-01-28
media_subpath: /assets/images
permalink: /2018/01/28/journey-to-vsan-a-technical-adventure/
categories: 
  - "Dell"
  - "VMware"
tags: 
  - "HCI"
  - "JourneytovSAN"
  - "VMware vSAN"
  - "VMware Vsphere"
  - "vSAN"
  - "vSphere"
  - "vSAN Licensing"
  - "VMware vSAN Editions"
  - "Technical Validation"
  - "vSAN ReadyNodes"
---

![](vSAN_Licensing.png)

Imagine having just configured some LUNs on your new PowerVault MD3820i.  Encryption key management has been configured, and 20 SEDs (self-encrypting drives) are spinning and ready for use with vSphere.  There are 4 SSDs in the PowerVault to use for caching that just need to be configured.

"What do you mean these cache disks aren't supported for use with SEDs?"  That was the question posed to Dell Support after being told we could not add cache disks for our SED LUNs in this array.

The SSDs were in the PowerVault, and the device recognized them without an issue.  We just couldn't configure them as cache. Even the documentation from Dell mentioned this configuration is not supported.  As it turns out, the SSDs we received were not SEDs and could not be used as cache in combination with SEDs.

While the SAN with SEDs would meet the corporate encryption requirements, losing caching capabilities meant the storage would no longer meet our IOPs needs.  And that is where our story begins.

After opening a support case and trying a firmware update with no change, the next call was made to our vendor's account team.   We were sold a solution that wasn't even supported, and we needed to do something different to meet our operational requirements.

The vendor team wanted to try and find some solid state SEDs to use for cache in the PowerVault before putting too much effort into engineering a different solution.  But their search for disks took a great deal of time.  After days and weeks of waiting with no drive to test, we pushed them to quote other options for a potential RMA of our current equipment while we began researching other options.

The boss suggested [Infinio](http://www.infinio.com/storage-acceleration) as a caching solution and asking the vendor to pay for it, so we began a trial.  Infinio is extremely simple to install in a vSphere environment, and their technical team is very good.  They walked us through the setup of the Infinio appliance, and we set it up to use host RAM for caching.  Then we did benchmarking using [diskspd](https://gallery.technet.microsoft.com/DiskSpd-a-robust-storage-6cd2f223) (the replacement for sqlio) inside different Windows VMs with Infinio activated to analyze the results.  While Infinio did a great job of caching disk read requests, it is not a write caching solution.  Despite the write cache increasing slightly, we were relying solely on the write cache of the MD3820i controllers when writing to disk.  The conclusion after testing was Infinio would not allow us to meet our IOPs requirements.

There were other storage device options, but all were more expensive.  Our budget was spent, and there was no going back to management for more of it.

I couldn't understand why none of our vendor reps had mentioned [VMware vSAN](https://www.vmware.com/products/vsan.html) as an option.  This was an avenue we hadn't considered.  I mentioned this to the boss, discussed the potential to use VM Encryption instead of buying SEDs, and advised looking at [ReadyNodes](http://vsanreadynode.vmware.com/RN/RN).

A few days later the idea was dismissed due to cost.  That didn't make sense to me, and we still had no solution.  At this point I picked the brains of several [community]({% link _posts/2017-11-18-the-power-of-finding-your-community.md %}) members to vet my idea from a cost standpoint.  One recommendation was to look at single-socket ReadyNodes to save on vSAN, vCenter, and vSphere licensing.  The servers would have an extra processor slot if we needed to add it later.  Our infrastructure as a whole was not CPU heavy, and of course the vendor had put two processors in the servers we ordered along with the PowerVault.

This licensing model spawned a series of new planning conversations.  There was talk of [VxRail](https://store.emc.com/en-us/Product-Family/VxRail-Products/Dell-EMC-VxRail-Appliance/p/VCE-VxRail?PID=EMC_SRS-VXRAIL-DC90_HERO) as an option, but it would have been too costly.  In the end, we found a solution that met our needs from a performance standpoint, spent no additional funds, and eliminated a single point of failure by using distributed storage.  See for yourself how the landscape changed.

The original BOM was:

- Three dual-CPU PowerEdge servers with no local storage
- PowerVault MD 3820i
- vSphere Standard for 6 sockets
- vCenter Standard
- Four PowerConnect switches

 The new BOM was:

- Four single-CPU PowerEdge servers (modified ReadyNodes), each with 2 disk groups (SSD for cache, HDDs for capacity)
- vSAN Standard for 4 sockets
- vSphere Enterprise Plus for 4 sockets
- vCenter Standard
- [Hytrust KeyControl](https://www.hytrust.com/products/keycontrol/) - 2 node license
- Two PowerConnect switches

### Lessons Learned

Technical validation is a big deal when making any purchase.  Sometimes the vendor doesn't get it right.  Sometimes we as IT buyers don't get it right.  When mistakes are made, consider all of the options before asking for more budget.  Be willing to work with your vendor (assuming you have a good relationship with them) to try and right the ship.   I guarantee both parties will learn and grow from the experience.

### Helpful Links

- [vSAN ReadyNode Configurator](http://vsanreadynode.vmware.com/RN/RN)
- [What You Can (and Cannot Change) in a vSAN ReadyNode](https://kb.vmware.com/s/article/52084)
- [Hybrid vSAN TCO and Sizing Calculator](https://vsantco.vmware.com/vsan/SI/SIEV)
- [All Flash vSAN TCO and Sizing Calculator](https://vsansizer.vmware.com/vsansizing/html/dcScaleDeploymentsOptions.html) (requires a [MyVMware](https://my.vmware.com/web/vmware/login?bmctx=4C976C546DE4E8BA7BD58B8EEADF25A5B418821E70E4480C483939EC36F11A86&contextType=external&username=string&OverrideRetryLimit=1&action=%2F&password=sercure_string&challenge_url=https%3A%2F%2Fmy.vmware.com%2Fweb%2Fvmware%2Flogin&creds=username+password&request_id=-4813831355726389842&authn_try_count=0&locale=en_US&resource_url=https%253A%252F%252Fmy.vmware.com%252Fgroup%252Fvmware%252Fhome) account to access all features)
- [If vSAN Powered The Matrix]({% link _posts/2017-09-04-if-vsan-powered-the-matrix.md %})
- [Architecting vSAN for Performance the VCDX Way](https://www.youtube.com/watch?v=I43mYaUDEvE)
