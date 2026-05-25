---
title: "Dell System Detect and the Case of the Missing USB Controller"
date: 2017-02-15
media_subpath: /assets/images
permalink: /2017/02/15/dell-system-detect-and-the-case-of-the-missing-usb-controller/
categories: 
  - "Dell"
tags: 
  - "Dell"
  - "Dell Latitude"
  - "Dell System Detect"
  - "Drivers"
  - "Windows 7 Pro"
---

Originally posted on [MangoLassi](https://mangolassi.it/topic/12577/dell-system-detect-and-the-case-of-the-missing-usb-controller) February 15, 2017

It was a normal day like any other. I was helping a user with some MS Office related issues via TeamViewer and was giving her laptop a tune up to maximize performance (things like disabling unnecessary startup items, cleaning out temp files, disabling MS Office add-ins, etc.). I thought I would go ahead and see if there were any driver updates to install. She was using a pretty new Latitude E5570 running Windows 7 Pro 64-bit.

I'm a fan of letting Dell's support site download Dell System Detect and auto-detect driver updates. There were about 6 driver updates needed (all "recommended" per Dell's support site), so we went ahead and installed them. I had warned the user the machine was going to reboot after the last one installed (a BIOS update).

The computer rebooted after all updates finished, and the user was able to login successfully. But, she had to step away from her computer for a while. Not long after this, she had to go to a remote site and do a presentation from her laptop. I got a text from her as she was on her way letting me know she couldn't get a USB mouse working with the laptop after the updates. The touchpad seemed to work fine, however. She made it to the remote site and tried a couple of different USB mice with no success. I jumped in just before the meeting with TeamViewer and picked up on the fact that Plug n Play did not seem to notice USB devices any longer. In fact, inside Device Manager, **there was no USB Controller**. It had vanished. You could scan for hardware changes in Device Manager, and the system would pick up on the fact that there was an unknown device connected but would bomb when trying to find a driver for it. Time ran short, and the user had to make due with just the touchpad for her presentation.

Fast forward a couple of hours when we get to do another screen share for me to try and solve this one. After all, I caused the problem in the first place (or so it seemed). She tried several other USB devices at her house with no luck (same symptoms as before). No amount of rebooting or shutting down that she had tried did anything to help. I couldn't find anything in the application or system event logs that was helpful but figured it had to be a driver issue. I tried reinstalling the chipset driver to see if that would do the trick, but that was not it. And then, it hit me in the face. There was a separate listing on Dell's support site for the Intel(R) USB 3.0 eXtensible Host Controller Driver. One could only suppose whatever version she had didn't install properly (even though we saw no indication of errors with the previous update). I installed the version of this driver previous to the most recent, and it worked like a champ. The USB Controller was back, and things began to work as expected. The version of the Intel(R) USB 3.0 eXtensible Host Controller Driver we used ended up being Version 4.0.0.36, A00 with the problem driver being Version 4.0.6.60, A04.

So, the moral of this story is, just because you can update drivers doesn't mean you should. And it doesn't mean the latest version of the driver will work like you think.
