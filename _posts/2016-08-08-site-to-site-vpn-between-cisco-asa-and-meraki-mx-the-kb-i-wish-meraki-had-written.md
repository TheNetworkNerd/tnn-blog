---
title: "Site-to-Site VPN between Cisco ASA and Meraki MX: The KB I Wish Meraki Had Written"
date: 2016-08-08
media_subpath: /assets/images
permalink: /2016/08/08/site-to-site-vpn-between-cisco-asa-and-meraki-mx-the-kb-i-wish-meraki-had-written/
categories: 
  - "Meraki"
tags: 
  - "Cisco"
  - "Cisco ASA"
  - "Cisco Firewall"
  - "IPSec"
  - "Meraki"
  - "Meraki KB"
  - "Meraki MX"
  - "Site-To-Site"
  - "VPN"
---

*Originally posted on MangoLassi August 8, 2016*
<!-- old link now no longer reachable is https://mangolassi.it/topic/10175/site-to-site-vpn-between-cisco-asa-and-meraki-mx-the-kb-i-wish-meraki-had-written -->

We lit up a new site earlier this year with Charter fiber and needed to connect it back to HQ. Then another site in our area needed to be connected back to HQ, presenting a firewall decision. Should we look to next generation Cisco ASA gear to replace our aging (and soon out of life) 5505s and 5510, look at a different type of product for a firewall, or look at UTMs as a viable option? Our network has been a hub and spoke for a while now with a 5510 at HQ and 5-6 other ASA 5505s out in the wild.

After much research and deliberation, we landed on Meraki MX gear. We got a MX84 for HQ and MX64s for the remote sites. This post is a little bit about the implementation and some hurdles we needed to jump to get the different gear working for site-to-site VPN capabilities to work as expected.

The plan was to take care of the spoke sites first, get all of the ASA 5505s replaced with MX64s, and connect them back to HQ's 5510 using IPSec. Then we'd replace the ASA 5510 with the MX84 and connect all sites again. I started reading up on this before we got the Meraki gear to prepare for what was coming. When deploying ASAs in the past, we had hired a consultant to do the configuration for us since none of us are Cisco proficient. I know enough to be dangerous within ASDM, but I cannot say the same from the command line. After several years in IT, I had never once tried to setup an IPSec tunnel on my own. This was the time. I'd save the company consultant fees for every device by tackling it myself.

Here's the KB from Meraki on creating a tunnel between Cisco ASAs and Meraki MX: [https://documentation.meraki.com/MX-Z/Site-to-site_VPN/Cisco_ASA_Site-to-site_VPN_with_MX\_Series](https://documentation.meraki.com/MX-Z/Site-to-site_VPN/Cisco_ASA_Site-to-site_VPN_with_MX_Series). That article is written for ASA version 8.3 and higher. We just happened to be on version 8.4(4)1 across the board, so things looked a little different. In any case, the directions were pretty easy to follow. Here's a click by click using ASDM in the version we had. The steps were similar to this and performed on our ASA 5510.

Go to Wizards -> VPN Wizard -> Site-to-Site VPN Wizard, and click Next to continue. ![](1.png)

Leave the VPN interface as outside, and enter the peer ip (which, in my case, was the WAN ip of one of the MX64 devices). ![](2.png)

Turn off IKEv2 since Meraki only supports v1. ![](3.png)

Identify local and remote networks. We liked using network objects in the ASA. ![](4.png)

Enter the pre-shared key for your tunnel. No device certificate is needed here. ![](5.png)

There is no need to change anything here. As the Meraki KB states,

> The MX security appliance can accept any of the following Encryption algorithms: DES, 3DES, AES-128, AES-192 and AES-256. Additionally the MX can accept either SHA1 or MD5 as the authentication hashing algorithm.

![](algorithms.png)

Be sure to check the option to exempt ASA side host/network from address translation, and leave it set to inside interface. ![](6.png)

Now you see the summary of the changes, so go ahead and click finish to setup the connection profile on the ASA side. ![](7.png)

As seen in the connection Profiles list... ![](8.png)

As we all know, sometimes using a wizard enables some options you don't want. At this point, I like to go to Configuration -> Site-to-Site VPN in ASDM and edit the connection profile. Once the edit profile window opens, expand Advanced from the left-hand tree, and go to Cryptomap Entry. Uncheck the option for NAT-T (since we have no other NAT device between the ASA and the MX). Click ok, and apply the changes. Be sure to save those to the startup configuration of the ASA as well. ![](9.png)

That's all that should be needed on the ASA side in terms of changes, so the rest we do on the Meraki MX side. This involves jumping into the Dashboard and setting up a Non-Meraki Peer (under Security Appliance -> Site-to-Site VPN on the Meraki network in question). We'll assume the public ip of the ASA is 2.2.2.2. Use the same pre-shared key for the tunnel as you entered on the ASA side. Save your changes, and wait a couple of minutes. ![](10.png)

If you start testing after making these changes to the MX, you will find that the tunnel connects, and you can send traffic between networks. It may even work for the better part of a day, but the tunnel will eventually drop unexpectedly. The root cause here is that the phase 1 and phase 2 negotiations for IKE / IPSec start to fail according to what you see in the Meraki event logs. But I followed the article. Everything should be fine, right? **Wrong.**

Here's another article Meraki links to at the bottom of that first article - [https://documentation.meraki.com/MX-Z/Site-to-site_VPN/Troubleshooting_Non-Meraki_Site-to-site_VPN\_Peers](https://documentation.meraki.com/MX-Z/Site-to-site_VPN/Troubleshooting_Non-Meraki_Site-to-site_VPN_Peers). Inside that article they finally tell you the default settings a MX uses when connecting with a 3rd party vendor's gear:

> Cisco Meraki devices have the following requirements for their VPN connections to non-Meraki peers: 
> - Preshared keys (no certificates). 
> - LAN static routes (no routing protocol for the VPN interface). 
> - IKEv1 (IKEv2 not supported) in Main Mode (aggressive mode not supported). 
> - Access through UDP ports 500 and 4500.

Go back to the ASA for a second, and dig into the connection profile you setup earlier. In the Basic settings, you see the IKE Policy list. Click the Manage button next to that to see a listing of all IKE policies. ![](IKE1Policies1.png)

If you highlight one of the polcies and choose to edit, you will see the default negotiation settings the ASA is using. ![](EditIKEPolicy.png)

At this point there are two options - change negotiation settings on the ASA side to match the Meraki MX, or change the Meraki MX negotiation settings to match the ASA side. I went with the latter option since I had the ASA 5510 connected to several 5505s and did not want to have to touch all of them.

Back inside the same Site-to-Site VPN area of Meraki Dashboard as before, click the Custom link under IPsec Policies. ![](10.png)

Once that opens, you can adjust all of the parameters so that the lifetime matches and the encryption and authentication settings for both settings match everything being used in your IKE Policies from the Cisco ASA. The settings below are what worked for me. ![](11.png)

Once these changes were made, the tunnel was solid. I learned this the hard way, so hopefully this can benefit someone else. I will also say that every MX device you want to connect back to a 3rd party device must be in Hub mode (can't just be in spoke mode). The non-Meraki peer you setup will be available to connect to any other MX devices in your Meraki Organization.
