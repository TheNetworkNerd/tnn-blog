---
title: "Amplifying Your vCenter Alarms"
date: 2018-08-06
media_subpath: /assets/images
permalink: /2018/08/06/amplifying-your-vcenter-alarms/
categories: 
  - "VMware"
tags: 
  - "vCenter"
  - "vCenter Alarms"
  - "vCenter Alerting"
  - "VMware vSphere"
  - "VMware vSphere 6.7"
  - "vSphere"
  - "vSphere 6.7"
image: "feature_alarm_1.png"
---

While some people may wake up each morning based on circadian rhythm alone, many of us require an alarm to wake up on time.  What does the alarm clock (or alarm app) really do for you?  It allows you to set a specific time at which a loud noise will happen.  The noise does not just happen for a split second.  The noise continues until you press a button to make it stop.  The noise will wake the sleeper because it is set to be in close proximity to him / her.

Let's break down that scenario further.  The alarm's noise is something you want to happen at a specific time and continue until manually stopped.  If you want the alarm to go off at 6 AM, for example, that specific time is a condition (or trigger) that will activate the alarm.  Based on the trigger being true (6 AM reached), the alarm activates, and an action is taken.  The action in this example is noise.  Pressing the button to turn off your alarm is an acknowledgement of the trigger and stops the alarm action from continuing.

### Alarms in the Datacenter

In the [vSphere](https://www.vmware.com/products/vsphere.html) world, an alarm answers the following questions:

- What object are we monitoring?
    - An alarm is created based on an **object** or object type (i.e. a datastore, a virtual machine, a cluster, a host, etc.).
- Under what conditions should the alarm activate?
    - The **trigger** is based on a condition or an event that occurs on the object for which the alarm is defined (i.e. loss of network connectivity to a host, etc.).
- What should happen when the trigger is true?
    - **Actions** are operations that occur in response to a trigger (i.e.  sending an e-mail, sending a SNMP trap, running a script, etc.).

There are a number of [pre-configured alarms in vSphere 6.7](https://docs.vmware.com/en/VMware-vSphere/6.7/com.vmware.vsphere.monitoring.doc/GUID-82933270-1D72-4CF3-A1AF-E5A1343F62DE.html) that the administrator can utilize.  Isn't it great all of these alarms get created automatically when upon deployment of vCenter?  I'll let you decide at the end of this article.

### Settings often Overlooked

A common mistake vSphere admins make is not realizing the only actions taken for all pre-configured alarms are an entry in the [vCenter event log](https://docs.vmware.com/en/VMware-vSphere/6.5/com.vmware.vsphere.monitoring.doc/GUID-4BCC37BC-58CB-45ED-AA3B-7CC79BCEDBBC.html) and a popup notifying the admin that an alarm has activated.  Unless one happens to be watching vCenter events like a hawk or at least has vCenter open constantly, how will you know when an alarm has fired?  As it now stands, you won't.

Every pre-configured alarm has actions an admin can specify.  Is taking an action always appropriate, and when it is, what type of action is merited?  How many alarms need to scream at you about defcon 1 level problems, and how many are you ok with knowing about the next time you login to vCenter?  The choice, my friend, is up to you.
 

### Adding Action to a Pre-configured Alert

Whether you have just deployed vCenter or have used it for years, some small tweaks can help you be more proactive.  As stated above, one popular alarm action is sending an e-mail.  Suppose you want to receive an e-mail when a host's CPU usage is high.  Here are the steps to make that happen:

- First, make sure vCenter is setup to send e-mail notifications per [this article](https://docs.vmware.com/en/VMware-vSphere/6.7/com.vmware.vsphere.vcenterhost.doc/GUID-467DA288-7844-48F5-BB44-99DE6F6160A4.html).

- Inside the vSphere Web Client, select vCenter in the navigator pane.  Then select Monitor -> Issues -> Alarm Definitions.  Here you can see all pre-configured alarms (all of which should be enabled) that apply at the vCenter level.  A different set of alarms would show up here if you had selected a cluster of hosts, for example. ![](1_HostCPUAlarm.png)

- As seen above, the Host CPU usage alarm is enabled.  Click the Edit button to see more detail.  In the General section, we see this alarm is monitoring host objects specifically.  ![](2_General.png)

- In the Triggers section, we see the conditions under which this alarm activates.  The alarm will activate as a warning condition if host CPU is above 75% for 5 minutes, but if the host CPU is above 90% for 5 minutes, the alarm will activate as a critical condition.  These thresholds can be edited, and more conditions can be added if you so choose.  The alarm should be activated when additional investigation might be needed. ![](3_Triggers.png)

- If we continue to the Actions section, one can easily see there are no actions configured for this alarm. ![](4_Actions.png)

- If you forgot to setup vCenter to send e-mail before making changes to an alarm, you will see a warning like this in the Actions section: ![](5_emailsender.png)

- Suppose we add an action to send an e-mail to adminteam@mydomain.com based on the previously defined triggers.  When the host CPU percentage increases from the warning threshold (75%) to the critical threshold (90%), the admin team will get an e-mail every 5 minutes while the trigger is true.  Then, once the host CPU percentage goes from critical back to warning, the admin team will only get one e-mail.  Click Finish to save the changes..  ![](6_EmailAction.png)

- Keep in mind this is just an example.  Many other options could have been configured here, including sending e-mails to different teams based on the different thresholds defined in our triggers. ![](7_AlarmDetails.png)

- Notice the alarm details now show the actions we specified.  Those actions only apply to this specific alarm.

 ### Quick Summary

Pre-configured alarms are a good thing, but they are only helpful when configured properly.  Don't be the person who sets an alarm but never knows it went off.  Take the time to configure actions for the alarms you as the vSphere administrator deem most critical to operating your datacenter to avoid being taken by surprise.  Check out the [vSphere 6.7 Monitoring and Performance Guide](https://docs.vmware.com/en/VMware-vSphere/6.7/vsphere-esxi-vcenter-server-67-monitoring-performance-guide.pdf) for everything you need to know to edit the pre-configured alarms, create your own, and master all things related to alarms.
