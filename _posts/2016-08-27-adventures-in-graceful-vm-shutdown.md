---
title: "Adventures in Graceful VM Shutdown"
date: 2016-08-27
media_subpath: /assets/images
permalink: /2016/08/27/adventures-in-graceful-vm-shutdown/
categories: 
  - "VMware"
tags: 
  - "APC"
  - "Graceful Shutdown"
  - "Opmonis"
  - "Power Outage"
  - "UPS"
  - "VMware"
  - "VMware ESXi"
---

*Originally posted on MangoLassi August 8, 2016*
<!-- original link now dead is https://mangolassi.it/topic/10487/adventures-in-graceful-vm-shutdown -->

#### The Problem

When working in SMB IT, budgets can be tight. You want to meet the needs of the business but also don't want to overspend. Taking that ideal solution and trimming it down to get in under budget but still meet business needs can be a challenge.

If you're going to have servers at your location and not inside a data center with clean, consistent, redundant power, you'll likely need a UPS (or perhaps more than one). The question is...did you select the right one to keep your infrastructure online long enough when the facility loses power to shutdown servers gracefully? And if the facility has a generator, does the UPS last long enough to keep things online until the generators kick in (which could be manual or automatic)?

Business leaders tend to think a UPS will solve all of the problems in a power outage. It's one thing to get a UPS, plug it in, and connect all of your rack equipment. Will the UPS now magically shutdown all your equipment when it gets low on juice? Without some additional effort, the battery is just delaying the same event which would happen if you didn’t have a UPS – everything gets powered off.

That doesn’t seem like a big problem until the power comes on again. Sometimes you get lucky, and everything works as it did previously. Sometimes you find the ERP system database or other important files have been corrupted. No matter what problems happen after this kind of power event, you get to fix it, and you’re still the one who overlooked getting those servers shutdown gracefully. Since we are likely talking about host servers running virtual machines, that could mean hundreds of VMs were just powered off.

#### A Project to Address the Problem

We recently deployed an ESXi host at one of our remote offices. We got a refurbished server through Xbyte for the job and a refurbished APC UPS through CoastTec. We did not add a network management card to the UPS.

The server is running ESXi 6.0U2 (vSphere Essentials) and a couple of VMs. I wanted to make sure we had something in place that would shutdown the host and its VMs if there was a power outage at the facility. I started looking at what APC had to offer as far as software goes - [https://community.spiceworks.com/topic/1652163-vsphere-6-0u2-vma-a-ups-and-graceful-vm-shutdown](https://community.spiceworks.com/topic/1652163-vsphere-6-0u2-vma-a-ups-and-graceful-vm-shutdown). In order to shutdown the host and its VMs, I’d have to deploy the vMA (vSphere Management Appliance) and install APC’s software on it. But, APC could not confirm they officially support ESXi 6.0U2 or if they even would.

#### Testing a Possible Solution

I posted on Twitter to see if anyone out there might have a solution that would work better and would officially support the version of vSphere we are running. I stumbled upon OPMONis - [http://opmonis.de/](http://opmonis.de/). Someone from their company contacted me, and we began discussing my use case.

It turns out OPMONis is a solution crafted by German developers that can monitor your UPS via USB cable and shutdown servers automatically in the event the battery power reaches a certain threshold (a percentage of total battery life that you specify or a specified number of minutes of battery life remaining). It can shutdown servers / workstations running Linux, Windows, ESXi hosts, client ESXi VMs, Free ESXi hosts, and even client VMs running on Free ESXi. They support a number of UPS vendors out of the box and versions of ESXi 4.5 or higher.

They have a 30-day free trial ([http://opmonis.de/en/licensing](http://opmonis.de/en/licensing)), so I decided to give it a try and see what happened.

Technically, the software could run on any Windows machine on a network as long as the UPS is connected to it via USB, but I chose to install it on a Server 2012 R2 VM running on the ESXi host mentioned in this post. I had to connect the APC UPS to my ESXi host via USB and pass that USB to the virtual machine that would be running OPMONis.

For best results in vSphere (screenshots here are from vCenter), it's best to power down the virtual machine first. ![](1_newdevice.png) 

Then, we add a host USB device to the virtual machine. This will add the USB device and a USB controller to the VM automatically. ![](2_controller.png)

In this case there was only one host USB device from which to choose, so it was selected by default. As soon as the change was confirmed and made in vCenter, I was able to see that once powered on again, the Windows VM recognized a battery was attached. ![](3_systemtray.png) ![](3A_systemtray2.png)

And now we install OPMONis. Downloading the trial is easy and requires no personal information be given before you download (which I liked). The installer is tiny. Here's a walk through of the install.  ![](4_install1.png)

Notice here you'll want the service and the client. OPMONis will run as a Windows service with automatic delayed start. ![](5_WindowsService.png) ![](6_FinishInstall.png)

Post-install, you can see OPMONis now running as a service. I'm guessing the service automatically gets a delayed start to make sure Windows has time to notice the UPS is attached. ![](7_postinstallservice.png)

Now, it is time to launch the OPMONis client to see what this software can do. ![](8_clienticon.png) ![](9_firstopen.png)

Upon first open, the first thing you want to do is add your UPS so OPMONis can start monitoring it. ![](10_UPSList.png)

If Windows can see the UPS device as a battery, then OPMONis should be able to see it in the list here to add and be monitored. ![](11_UPSDeviceDetect.png)

As you can see here, OPMONis recognized the APC UPS, shows it is 100% charged, and shows that I have 59 minutes of uptime based on the power usage of the connected equipment. In my case, the shutdown threshold is 15 minutes of time remaining. Once we're down to 15 minutes of juice left, OPMONis will shutdown my equipment. ![](12_UPSListUpdated.png)

But wait! I never added any equipment to be shutdown. From the main OPMONis menu, go to the System Administration settings: ![](13_settings.png)

At the moment, there are no systems setup to be shutdown by OPMONis, so we click the plus sign to add a system. ![](14_NoSystems.png)

As you can see, I can add a Windows machine (physical or virtual), a Linux machine (physical or virtual), a paid ESXi host, a Free ESXi host, a paid ESXi Client VM, or a Free ESXi Client VM. ![](15_ESXihost.png)

I started off thinking I should add each of my virtual machines individually as ESXi Client VMs, but by the time they shutdown, OPMONis wouldn't be able to shutdown the host too because the VM running OPMONis would be offline.

I decided to add my ESXi host and do some testing to see how this would work. Of course, you will need a user with permissions to issue a host shutdown command over the network (assuming you have the correct firewall ports open on your ESXI host to allow this). ![](16_shutdownconfig.png)

You see that check box marked "Await Execution?" If you add several systems for shutdown and order them as desired, you can check the Await Execution box and have OPMONis wait for one machine to shutdown before proceeding to the next one in the list.

Suppose we add a Windows machine to the list (as in OPMONis will attempt to shut it down via WMI): ![](17_WindowsVM.png)

As you can see from the screenshot of DFWESXi1, Await Execution was not checked. That means OPMONis will, once the battery threshold is reached, attempt to shutdown both of these machines at the same time. Beware of that option as I believe it is checked by default when you add a new system to the list in this area.

For the purpose of testing, I removed all systems except for the ESXi host in question. Notice from the Systems Administration menu that I'm currently operating in Automatic mode. I can click the settings button to switch to manual mode and make OPMONis try to shutdown everything in my list of systems for testing purposes. ![](18_SystemsList.png)

When you switch to manual mode, you get asked to confirm that you really want to switch to manual mode before the change is made. Once confirmed, here is what you see: ![](19_Manualmode.png)

So then I tested, and tested, and tested again...until I got it right. There's something you have to remember about ESXi hosts, especially when you are not in a HA cluster. Each host has virtual machine startup / shutdown settings (go to the host in vCenter -> Manage -> VM Startup Shutdown or Configuration -> Virtual Machine Startup / Shutdown in the vSphere Client). Pay very, very close attention to the shutdown action for your VMs, making sure it is set to Guest Shutdown: ![](20_guestshutdown.png)

That's the first step. By default, vSphere seems to use a shutdown delay of 120 seconds per VM. That means if you were to issue a shutdown command to the host using the vSphere Client, the host would try to shutdown the guest OS of each VM in your Startup / Shutdown settings for 2 minutes before just powering them off. If the guest OS shutdown of a VM takes less than 120 seconds, the host will proceed to shutdown the guest OS of the next VM.

What I did was run the manual shutdown using OPMONis while connected to the host with the vSphere Client. Performing this manual shutdown while watching the events on the ESXi host is a great way to see when each VM gets a command to shutdown the guest OS, when the VM is confirmed to be powered off, and when the next guest OS shutdown starts. Since I was running the vCenter Server Appliance as a VM on this host, I found it actually took longer than 2 minutes to shutdown and had to manually specify a longer shutdown delay for that VM so it could completely shutdown gracefully before the host force powered it off. **Make sure you know how long it takes for your VMs to shutdown gracefully if shutting down an ESXi host with OPMONis.** ![](21_shutdownlist-1024x302.png)

#### How OPMONis Met This Need

Overall, I really like the OpMonis software. Their Small Business version allows you to use the software for up to 10 devices on your network. They can be workstations or servers from what I understand. You might be saying to yourself that 10 devices is not many. But if one of them is an ESXi host and can shutdown all VMs on that host for you, I'd say only counting that as one device is pretty awesome.

I think folks who only run free ESXi could really benefit from using this as well. I'm not sure if the proprietary software from other vendors can actually shutdown those hosts gracefully like OPMONis can.

You can add multiple UPS devices to be managed by the software, and I would hope you can add a different list of servers to shutdown for each UPS added. I was not able to test that one.

You might be wondering why I chose to use OPMONis Small Business Edition (which I actually ended up purchasing) instead of sticking with APC's proprietary software. I paid a one time fee for this software that can be used with many different UPS devices. If my UPS fails, and I get a different brand, I already have OPMONis at my disposal and don't need to learn and test a new software solution to shut down my servers. I'm not managing an additional VM. It was installed on an existing VM and could have been installed on a workstation directly connected to the UPS if I had wanted. And of course, if you just don't like the proprietary software that you can use to manage the UPS you bought, OPMONis is another option.

#### Desired Future Feature Set

I think for a software that's early in its development, they have a great foundation. I know for a fact they will be introducing SNMP support in future releases. Here's a wish list for other things I'd like to see:

- Ability to monitor a UPS over the network (available as of [OpMonis 1.2.2 Release](http://forum.opmonis.de/thread/opmonis-release-1-2-2-is-available/))
- E-mail notifications when your battery kicks in and just before a shutdown event is kicked off
- Ability to select more than one ESXi Client VM at once to shutdown rather than adding one at a time (maybe a GUI that allows you to point at your host or vCenter and select the VMs you want to shutdown automatically). I'm still not sure why you'd only want to shut down a few VMs instead of all of them on a host, but you could choose the ESXi Client VM option for every VM on your host if you need to make sure some VMs shutdown before others (i.e. the Await Execution option).
- Hyper-V support (available as of [OpMonis 1.3.0 Release](https://mangolassi.it/topic/11998/opmonis-1-3-0-is-available-with-hyper-v-and-xen-support))

> As of May 2017, OpMonis supports ESXi 6.5 free and paid hosts. Read the release notes for version 1.3.3 [here](http://forum.opmonis.de/thread/opmonis-release-1-3-3-is-available/).
