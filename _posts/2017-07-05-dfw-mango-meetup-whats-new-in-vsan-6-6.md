---
title: "DFW Mango Meetup - What's New in vSAN 6.6?"
date: 2017-07-05
media_subpath: /assets/images
permalink: /2017/07/05/dfw-mango-meetup-whats-new-in-vsan-6-6/
categories: 
  - "VMware"
tags: 
  - "MangoLassi"
  - "VMware vSAN 6.6"
  - "VMware vSphere"
  - "VMware vSphere 6.5"
  - "VMware vSAN"
  - "vSAN"
  - "vSAN 6.6"
---

With the GA of vSAN 6.6 in late April, I was asked to give a talk at the DFW Mango Meetup back in May about all the new features in this release.  The power actually went out where we were presenting, so I had to use my laptop to present with no projector.  You can find the full presentation video [here](https://youtu.be/uw_-HZHX90c?si=-lVjj0QgnNkHqT5W) along with some corrections below.

<!-- original video link - https://www.youtube.com/watch?v=uw\_-HZHX90c -->

Here are some corrections to the video:

- Video at 3:00 - The vSAN ROBO licensing lets you license vSAN on a per-VM basis in packs of 25.  These can span multiple locations, but the hard cap is you cannot have more than 25 VMs per physical location (requires a Standard / Advanced / Enterprise license at that location).  Read the [vSAN 6.6 Licensing Guide](https://www.vmware.com/content/dam/digitalmarketing/vmware/en/pdf/products/vsan/vmware-vsan-66-licensing-guide.pdf) carefully on that one.

- Video at 5:14 - I failed to mention [vSAN ReadyNodes](http://vsanreadynode.vmware.com/RN/RN) when talking about hardware needed to run vSAN as an alternative to building your own server configuration.  The ReadyNodes are pre-configured and easily obtained through your reseller of choice.

- Video at 6:20 - I mentioned an accidental unsupported upgrade.  The details of that can be found [here]({% link _posts/2017-05-05-tales-of-a-vcenter-upgrade-gone-wrong-journey-to-vsan-6-6.md %}).  The rule of thumb when upgrading to a newer version is always check the release notes, and VMware does not support upgrades to a higher version of vSphere released earlier in the calendar year (i.e. 6.5a released chronologically before 6.0U3).

- Video at 9:15 (see slide 5 in the PowerPoint) - I mentioned Flash Read Cache when discussing the default vSAN storage policy, but this is NOT referring to VMware's Flash Read Cache (separate product).  The flash read cache reservation percentage listed in the default storage policy is the portion of cache (so space on your SSDs for caching) that you want reserved for reads.  The only real use case I see for this is if you had a set of VMs with their own customized storage policy for a read-intensive database application.  My thinking is if you sized the cache correctly, you probably would not need to reserve any of it for reads.  But the option is there.  If anyone does reserve part of the cache, I would love to hear the use case.

- Video at 15:40 - I didn't specifically call out the fact that an external KMS is required for vSAN Encryption just like with VM Encryption, but the two are quite different in the way encryption is applied.

Additional Resources:

- [VMware HCL - specifically for vSAN components](https://www.vmware.com/resources/compatibility/search.php?deviceCategory=vsan)
- [vSAN 6.6 Licensing Guide](https://www.vmware.com/content/dam/digitalmarketing/vmware/en/pdf/products/vsan/vmware-vsan-66-licensing-guide.pdf)
- [vSAN Ready Node Configurator](http://vsanreadynode.vmware.com/RN/RN)
- [VMware StorageHub](https://storagehub.vmware.com/) - lots of great vSAN documentation here
- [Duncan Epping's Blog](http://www.yellow-bricks.com/)
- [John Nicholson's Blog](http://thenicholson.com/)
- [The Virtually Speaking Podcast](https://soundcloud.com/virtuallyspeakingpodcast)
