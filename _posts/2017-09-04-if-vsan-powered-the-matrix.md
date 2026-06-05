---
title: "If vSAN Powered The Matrix..."
date: 2017-09-04
media_subpath: /assets/images
permalink: /2017/09/04/if-vsan-powered-the-matrix/
categories: 
  - "Conferences"
  - "VMware"
tags: 
  - "JourneytovSAN"
  - "vBrownBag"
  - "VMware"
  - "VMware vSAN"
  - "VMworld"
  - "VMworld 2017"
  - "VMworld US"
  - "VMworld US 2017"
  - "vSAN"
  - "VMware vSAN"
  - "VMworld Conference"
  - "Technology Conferences"
  - "Tech Conferences"
---

![](WhatIfIToldYou.jpg)

After finding out I was approved to attend VMworld this year, I signed up to present a vBrownBag Tech Talk on VMware vSAN.  Just a few weeks before the conference, I was lucky to secure an official slot on the agenda for Thursday afternoon.

The idea for the talk was pretty simple.  If vSAN powered The Matrix, would we plan it carefully like an enterprise technology rather than trying to run it on whatever hardware is available in the server closet?  Is cost really a barrier to people using this technology, or are there ways to save money and still harness the power of VMware vSAN?  What are some other pitfalls to be aware of?  [Watch the video](https://www.youtube.com/watch?v=EHJlHPu6h0Y), and see for yourself (session VMTN6733U).  I hope you will find it helpful.

{% include embed/youtube.html id='EHJlHPu6h0Y' %}

 

Here are some corrections / additions to the video:

- Video at 2:31 - I mentioned [Starwind Virtual SAN](https://www.starwindsoftware.com/starwind-virtual-san) in this part of the video.  This is a software solution that will run on top of VMware or Xen as a nested VM or as a native application on Hyper-V and Windows.  It's slightly different than VMware's vSAN but still a very interesting product that can provide extreme performance for many different types of workloads.

- Video at 3:20 - I don't think I ever mention in relation to this slide that in the VMware vSAN realm we are turning off RAID on the physical hosts and letting the hypervisor pool the capacity drives from disk groups all hosts to make up a vSAN datastore.  The RAID level (1, 5, 6) describes the way the VMDK objects are protected (number of components of a VMDK object determined by RAID level plus witness).

- Video at 9:30 (or thereabouts) - keep in mind we want all hosts in our vSAN cluster to have the same hardware configuration (same processor family and number of processors, same amount of RAM, same number and capacity of disks, same number of disk groups, etc.).

- Video at 12:50 - stretched clusters pool storage in hosts at multiple sites to make up a single vSAN datastore that can be seen and managed in vCenter.  Bandwidth requirements do apply in these cases (see the slide deck or [VMware StorageHub](https://storagehub.vmware.com/) for more).

- Video at 16:00 - in the 4 node RAID 1 configuration, one node failing allows for an additional copy of the component that was on the failed host to be created on one of the other hosts so all VMs are still compliant with the storage policy.

 

### Additional Resources

- [vSAN Terms and Definitions](https://docs.vmware.com/en/VMware-vSphere/6.5/com.vmware.vsphere.virtualsan.doc/GUID-1D8956A2-3F46-49C8-9231-38F3A9D09A0F.html)
- [vSAN Cache Sizing](https://blogs.vmware.com/virtualblocks/2017/01/18/designing-vsan-disk-groups-cache-ratio-revisited/) - also check VMware StorageHub for the latest documentation and sizing guides
