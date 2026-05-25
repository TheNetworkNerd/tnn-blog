---
title: "If vSAN Powered The Matrix, Extended Edition"
date: 2018-10-24
media_subpath: /assets/images
permalink: /2018/10/24/if-vsan-powered-the-matrix-extended-edition/
categories: 
  - "Conferences"
  - "VMware"
tags: 
  - "Blogtober"
  - "JourneytovSAN"
  - "VMUG"
  - "VMUG UserCon"
  - "VMware"
  - "VMware vSAN"
  - "vSAN"
image: "matrix-3109795_640.jpg"
---

The presentation featured in this post is from the October 2018 Dallas VMUG UserCon and is the extended edition of an idea born from a [vBrownBag presentation at VMworld 2017]({% link _posts/2017-09-04-if-vsan-powered-the-matrix.md %}).  After giving that presentation in 2017, I quickly realized there was enough content to make it a full session.  As I allude to in the video, this talk has been given by others at VMUG UserCons across the country throughout this year.  But since this has been my first and only attempt at it, I have the full video and newly developed slide deck for anyone who wants it.

 

Here are some corrections / additions to the video:

- 2:30 - If you've never seen the red pill / blue pill scene from [The Matrix](https://www.imdb.com/title/tt0133093/), check it out [here](https://www.youtube.com/watch?v=ytftrd6rxps).
- 3:44 - vSAN is object-based storage.  Dig into vSAN terms and definitions [here](https://docs.vmware.com/en/VMware-vSphere/6.5/com.vmware.vsphere.virtualsan.doc/GUID-1D8956A2-3F46-49C8-9231-38F3A9D09A0F.html).
- 5:43 - What is a [fault domain](https://docs.vmware.com/en/VMware-vSphere/6.5/com.vmware.vsphere.virtualsan.doc/GUID-FE7DBC6F-C204-4137-827F-7E04FE88D968.html#GUID-FE7DBC6F-C204-4137-827F-7E04FE88D968)?
- 8:00 - The [default storage policy](https://docs.vmware.com/en/VMware-vSphere/6.7/com.vmware.vsphere.virtualsan.doc/GUID-C228168F-6807-4C2A-9D74-E584CAF49A2A.html) for a vSAN cluster is RAID 1 with FTT = 1.
- 9:33 - Go take a look at the slide showing the read / write magnification of using RAID 1 vs. RAID 5 vs. RAID 6 storage policies (slide 8).
- 13:12 - It's noteworthy to restate that VM Encryption is a capability you get with vSphere Enterprise Plus, while vSAN Encryption is part of vSAN Enterprise.  Each functionality requires an external KMS.  Check the [VMware Compatibility Guide](https://www.vmware.com/resources/compatibility/search.php?deviceCategory=kms) for a supported KMS vendor and product version.
- 14:12 - Check out [this episode](http://www.vspeakingpodcast.com/episodes/77) of the Virtually Speaking Podcast to hear Mike Foley talk about FIPS compliance.
- 16:12 - Check out [this post](https://blogs.vmware.com/virtualblocks/2018/04/20/vsan-6-7-vrealize-operations-within-vcenter/) for full details on the vROps dashboards mentioned in the video.
- 17:12 - A remote or branch office is not the same as your datacenter!
- 23:00 - There is vSAN Network Design help on [StorageHub](https://storagehub.vmware.com/t/vmware-vsan/vmware-r-vsan-tm-network-design/).
- 29:00 - Check out the [vSAN Design and Sizing Guide](https://storagehub.vmware.com/t/vmware-vsan/vmware-r-vsan-tm-design-and-sizing-guide-2/), which includes a walkthrough of the [vSAN Sizer Tool](https://storagehub.vmware.com/t/vmware-vsan/vmware-r-vsan-tm-design-and-sizing-guide-2/vsan-sizing-tool/).
- 35:00 - Stick to simple HBAs instead of feature rich controllers.  We're not doing hardware RAID.
- 35:30 - The vSAN Sizer tool does take into account memory overhead on all hosts for using vSAN.
- 36:50 - There is support through VUM for updating storage controller versions for a limited number of OEMs at present.
- 38:35 - The problem truly is choice.  The way you consume vSAN is up to you and depends on how and where you want to spend your time.  Keep in mind no choice will produce successful outcomes without proper planning.  Some may call these additional consumption options for vSAN the equivalent of taking the blue pill (which is never addressed point blank in the video).  In my opinion, though the depth of knowledge required may vary based on choice of consumption model, some knowledge of vSAN will be required in each case.

 

 

https://youtu.be/sNKQEdgapwM

 

### Additional Resources

[If vSAN Powered The Matrix - Full Slide Deck](http://34.42.71.182/wp-content/uploads/2018/10/NK_DFWVMUG_October2018_If-vSAN-Powered-The-Matrix_FullSideDeck.pptx)

[Nerd Journey Podcast](http://nerd-journey.com/)
