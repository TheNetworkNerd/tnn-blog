---
title: "Error Deploying Hytrust OVA in the ESXi 6.5 Embedded Host Client and Learning to Read"
date: 2017-07-17
media_subpath: /assets/images
permalink: /2017/07/17/error-deploying-hytrust-ova-in-the-esxi-6-5-embedded-host-client-and-learning-to-read/
categories: 
  - "VMware"
tags: 
  - "Embedded Host Client"
  - "ESXi"
  - "FreeBSD"
  - "HyTrust"
  - "Intel"
  - "OVA"
  - "Physical Address Extensions"
  - "VMware ESXi"
  - "VMware vSphere"
  - "VMware vSphere 6.5"
  - "vSphere"
---

Have you ever thought you found a bug in a software product?  In my case, I was almost certain I had found one but learned the problem was me.  I want to share a story from this past week that helped me learn to read a bit more carefully before reporting a bug and some interesting observations made as a result of my mistakes.  Hopefully this will help someone.

### Some Background Information for the Reader

We were trying to deploy a 2-node Hytrust cluster to use with vSphere VM Encryption.  I had downloaded the OVA for VMware, and we were able to deploy the first Hytrust node to a datastore in our vSphere 6.5d cluster using vCenter with no problems.  We had a couple of additional hosts not managed by vCenter and were ready to deploy the second Hytrust node on one of those hosts. Since the hosts outside the cluster were running ESXi 6.5d as well, our only choice for deployment of the OVA was using the ESXi embedded host client.  Here are the steps I followed:

- Login to the ESXi embedded host client at https://ipaddressofhost/ui/#/login.

- Click the option to Create / Register VM.  
![](1_Create.RegisterVM.png)

- The creation type needs to be Deploy a virtual machine from an OVF or OVA file.  
![](2_DeployfromOVForOVA.png)

- Select the OVA file, and make sure it is recognized by the embedded host client.  
![](3_ChooseOVAFile.png)

- Select the storage on which the OVA will be deployed.  In my case it was a local datastore.  
![](4_SelectStorage.png)

- Soon after clicking Next from the menu above, option 4 changed from License agreements to Deployment options as seen here.  I thought that was a little odd but kept going with the deployment.  
![](5_ChooseNetwork.png)

- After clicking Next on the screen above, I was immediately met with the following error:  
![](6_NextafterDeploymentOptions.png)  

For purposes of searchability, here's the full text error:

> _Unfortunately, we hit an error that we weren't expecting.The client may continue working, but at this point, we recommend refreshing your browser and submitting a bug report.Press the Esc key to hide this dialog and continue without refreshing._
> 
> When I clicked on the details button, here's what I saw:  
> ![](7_ErrorDetails.png)
> 
> Again for searchability:
> 
> _Cause: TypeError: Cannot read property 'replace' of null_  
> _Version: 1.18.0_ _Build: 5270848_  
> _ESXi: 6.5.0_   
> _Browser: Mozilla/5.0 (Windows NT 6.3; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/59.0.3071.115 Safari/537.36_  
> 
> _Exception stack:_
> 
> _TypeError: Cannot read property 'replace' of null_  
> _at https://192.168.10.36/ui/scripts/main.js:368:10583_   
> _at Object.r \[as forEach\] (https://192.168.10.36/ui/scripts/main.js:318:21020)_   
> _at https://192.168.10.36/ui/scripts/main.js:368:10342_ _at Object.r \[as forEach\] (https://192.168.10.36/ui/scripts/main.js:318:21126)_   
> _at getPropertyFields (https://192.168.10.36/ui/scripts/main.js:368:10099)_  
> _at new <anonymous> (https://192.168.10.36/ui/scripts/main.js:368:12872)_   
> _at Object.e \[as invoke\] (https://192.168.10.36/ui/scripts/main.js:319:3874)_  
> _at w.instance (https://192.168.10.36/ui/scripts/main.js:319:23885)_  
> _at https://192.168.10.36/ui/scripts/main.js:319:15273_  
> _at r (https://192.168.10.36/ui/scripts/main.js:318:21126)_  

### Maybe This is a Bug?

I thought at first maybe it was a fluke, so I reloaded the embedded host client on this host and tried to deploy again, which led me to the same error.  I tried the other host outside the cluster and was met with the same error.  I tried with Chrome, IE, and even Firefox and was met with this error every time.  But the odd part was I knew the OVA was good because we already deployed it once with vCenter inside our cluster with no problems.  I even tried a previous version of the OVA using the same steps and again received the error mentioned previously.

At this point, I thought I'd try the C# vSphere Client just for fun to see if it worked for the deployment (even though not supported).  I was able to open the C# client and select the option to deploy the OVA and even made it to the screen where you're prompted for network settings as seen here.  I didn't actually complete the deployment in the C# client, however.   

![](8_DeployvsphereClient.png)

I thought based on this the hosts could actually read the OVA and that there must be a bug in the embedded host client, especially since I had tried on multiple hosts and in multiple browsers.  At this point I put in a VMware Support ticket describing the issue.

### Making Friends with Technical Support

When the technician called, I showed him the deployment failed inside the embedded host client in different browsers and on different hosts.  He had me try from a different computer, and we were met with the same error.  Next, he had me (using the embedded host client) create a very small VM with no OS installed and export it to OVF.  We were able to immediately import that OVF template and use it to create a new VM.  Based on that, he did not seem to think the issue was the embedded host client.  When I mentioned the C# client seemingly working for deployment, he said it was very inconsistent (even in their own internal labs) as to when the old C# client would actually work to connect to a 6.5 host and when it would not work.  I didn't complete the deployment in the C# client since it isn't supported, and I truly wanted to know what the root problem was with the steps I followed.

We talked about the OVA being for Hytrust.  And then the technician pointed out something that made me feel like a complete idiot for wasting his time.  On page 22 of the install guide for Hytrust v4.0, there was verbage in the OVA installation section which specifically says the OVA will only work if you are using vCenter.  If you're using a host not managed by vCenter, you must deploy Hytrust using their ISO.  

![](9_HytrustGuidep22.png)

I'll admit I even had this PDF open on my computer and had somehow missed the statement mentioned here.  The VMware Support technician then went above and beyond his calling.  He stayed on the phone with me while I downloaded the ISO and deployed it on my standalone host via the embedded host client to make sure it worked.  I thanked him for his time and told him I was still learning to read.  Every time I have had to contact VMware Support, it has been a wonderful experience, and this was no exception.

### Lessons Learned and Missing Components of Hytrust's Guide

The Hytrust ISO can be deployed inside a new VM much like a Windows ISO (upload to datastore and attach as CD-ROM of the VM), but there are some differences here.  Hytrust gives you some guidelines for CPU, RAM, and disk space in their install guide, but they give no guidance on what virtual NIC to use (E1000, VMXNET3, etc.), which operating system to choose when you create the VM, or whether VMware Tools has to be installed separately.  I didn't really want to have to guess, but I did have an already deployed Hytrust node running inside my cluster to reference.  After referencing the Hytrust node VM we had deployed through the OVA, here's what vCenter reported:

- Guest OS:                  FreeBSD (64-bit)  - _read my closing thoughts below before using this OS_
- VM Compatibility:  ESXi 5.0 and later (VM version 8)
- VMware Tools:        version: 2147483647 (Guest Managed)
- Network Adapter:   VMXNET3
- SCSI Controller:      LSI Logic Parallel
- vCPU:                        2
- RAM:                         8 GB
- Disk Size:                 20 GB (thin-provisioned)

Here's a shot of the exact options you need to select so that FreeBSD (64-bit) can be your operating system choice inside the embedded host client.

![](10_FreeBSDSettings.png)

After selecting the options mentioned here, I was able to deploy that second Hytrust node without any issues using the ISO and follow the steps in the guide to join it to the first node.

 

### Closing Thoughts

One last item of interest is the fact that vCenter 6.5d recognizes the guest OS of the Hytrust VM deployed via OVA as FreeBSD (64-bit).   The embedded host client on the host where the second node lives shows that it believes FreeBSD 32-bit is the OS.

![](11_32-bitGuest.png)

Could I change the guest OS for the VM?  Indeed I could.  But, if the VM were running only the 32-bit version of the OS, I was not sure why Hytrust's guide would recommend using 8 GB of RAM for your VM in a standard installation and 16 GB in a large installation.  Hytrust VMs use SecureOS, and you cannot really see anything about the underlying FreeBSD operating system unless you reboot the VM.  I thought maybe it was the version of VMware Tools Hytrust has inside their ISO and OVA that isn't reporting the OS to VMware correctly.

But I wasn't willing to stop there.  When I rebooted one of the Hytrust VMs and watched the VMware Remote Console, I saw the underlying OS was FreeBSD 32-bit.  Upon further reading of [https://www.freebsd.org/doc/en/books/faq/hardware.html](https://www.freebsd.org/doc/en/books/faq/hardware.html), I realized the 32-bit version of FreeBSD is actually being used (in the case of both the OVA and the ISO deployment).  As it turns out, the processor in the hosts inside my cluster and in the host outside the cluster running Hytrust were [Intel Xeon E5-2650 v4](http://ark.intel.com/products/91767/Intel-Xeon-Processor-E5-2650-v4-30M-Cache-2_20-GHz), and as you can see from the link, these processors supported PAE (physical address extension).  That means the 32-bit version of FreeBSD can use more than 4 GB of RAM.

But it was still interesting to me that vCenter reported the OS as FreeBSD 64-bit, while both ESXi hosts running Hytrust VMs show the OS is really FreeBSD 32-bit.  I powered down each VM, edited the settings of the VM (VM Options tab -> General Options), and changed the OS to FreeBSD 32-bit.  Each VM booted up with no issues, and my Hytrust cluster is working fine for now.

If you ask me, there's a bug in vCenter's ability to detect FreeBSD 32-bit as a guest OS.  :)
