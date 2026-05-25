---
title: "Captain vSAN and the Case of the Cluster License Mystery"
date: 2017-12-11
media_subpath: /assets/images
permalink: /2017/12/11/captain-vsan-and-the-case-of-the-cluster-license-mystery/
categories: 
  - "VMware"
tags: 
  - "Journeytovsan"
  - "VMware Licensing"
  - "VMware vSAN"
  - "VMware vSAN 6.6"
  - "VMware vSAN Licensing"
  - "vSAN"
  - "vSAN 6.6"
image: "Captain_vSAN.png"
---

Excitement was in the air for the technology team of Beast Mode, Inc.  After months of planning, some shiny new [vSAN ReadyNodes](http://vsanreadynode.vmware.com/RN/RN) finally arrived at the datacenter.  The implementation team gathered to rack and cable the new cluster and all other necessary equipment.  Even though ESXi was installed on the hosts with basic network settings configured, there was no datastore on which to install vCenter.  They had yet to configure the vSAN cluster.

A bit of worry set in, but John, the team lead, remembered something.  As part of the vCenter Server Appliance installer in vSphere 6.5d and later (vSAN 6.6 and later), they could leverage [Easy Install](https://storagehub.vmware.com/export_to_pdf/easy-install) to create a vSAN datastore on some of the hosts and use it to deploy vCenter.  With vCenter online and hosts updated to the latest patch of ESXi 6.5, they configured the rest of the hosts in the vSAN cluster with all networking needed for vSAN traffic, management, and vMotion.  Now that the cluster was fully functional, the team was ready to deploy some virtual machines...or so they thought.

John also reminded the team to apply the VMware licenses before going any further.  The cluster in question contained 4 hosts, each with a single physical processor, fully licensed for vSphere Enterprise Plus and vSAN Standard.  Each host would be managed by vCenter Standard.  The team knew how to apply the vSphere licenses as well as the vCenter license.  But the mystery began when they tried to apply their licenses to use vSAN.

"Unlike vSphere licenses applied to each host individually, a vSAN license must be applied to the cluster as a whole," John said.  "Any host moved into the cluster will then get the vSAN license applied by default."  The screenshot here shows the cluster was using 4 CPUs of vSAN (contained all of their ReadyNodes) and was still operating in evaluation mode.

![](1_Evaluation_Mode_Features_in_vSAN_Cluster.png)

When the licenses for vSAN were originally issued, the team was sent 4 different 1 CPU license keys for vSAN.  After clicking the Assign License button above and entering all license keys, here's what the team saw.  Notice the evaluation license is still assigned to the cluster.  The other 4 licenses have been added as options to assign to the cluster.

![](6_Assign_License_4Available.png)

When the team selected one of the 1 CPU vSAN Standard licenses to assign to the cluster, it was easy to see the Usage column showed 4 CPUs but that Capacity was only 1 CPU.  They were thwarted by errors about insufficient license capacity.

![](8_LicensedCapacityInsufficient.png)

At this point, clicking OK didn't help matters.

![](9_LicensedCapacityInsufficient_Error.png)

John and the rest of the team were getting frustrated.  He couldn't bear the thought of telling the boss they were stuck at such a seemingly simple step.  He thought to himself, "why would the reseller or VMware send licenses that cannot be used?  If only Captain vSAN were here, he could help us."  And in a quiet voice he spoke the words to summon Captain vSAN to their aid.  "Help us, Captain vSAN.  By the power of the SDDC, we summon you to our aid."  In in a flash, Captain vSAN appeared!

The team heard it before they saw him.  "Who dare summon Captain vSAN?"  This was a moment many an IT pro had only dreamed of but that few had experienced.  What do you say when coming face to face with a legend?  The team had to think on their feet.  They had to let the Captain know they didn't summon him for nothing.  Henry was the one to speak up.  "We've come so far, Captain.  Our cluster is humming along beautifully, but applying the license isn't working.  Can you help us?"

His voice filled the room.  But it was calming and not angry, sympathetic and not critical.  He immediately put the team at ease.  "Of course I can help you.  I am Captain vSAN, solver of any vSAN problem, evangelist of the software defined datacenter, hero of IT pros everywhere.  Fear not, my friends.  These licenses are no match for us.  Take a deep breath, and let's go on an adventure together."

### The Journey to myvmware.com

"Henry, please login to your myvmware.com account for me," said Captain vSAN.  "We need to tweak the licenses just slightly to resolve this cluster license issue.  After you login, go to the Manage License Keys area.  Make sure you have selected the vSAN 6 product from your product list, and choose the option to 'View License Keys.'  Notice you see the 4 separate 1 CPU license keys that have already been entered in vCenter."

![](10_MyVMwareTotalof4.png)

![](11_MyVMwareTotalof4_Detail.png)

 

"Change the dropdown that says 'View License Keys' so that 'Combine License Keys' is selected, Henry."

![](12_CombineLicenseKeys.png)

"Select all 4 of those 1 CPU licenses, and click Continue.  We're going to combine those keys and generate a new license key with a usage capacity of 4 CPU."

![](13_Combine4Keys_1.png)

"It looks like we can enter a note here," said Henry.  "I'll go ahead and do that so anyone looking in our account knows what happened before I click Confirm."  The captain smiled.  "Now you're getting it," he said.

![](14_Combine4Keys_3.png)

 

"Just confirm you have read and understand the warning, Henry.  Then click Continue."

![](15_Combine4Keys_2.png)

 

Henry was excited.  "I see our new key!  It looks like we can even e-mail this for our records if we want.  I'll go ahead and click Ok."

![](16_Combine4Keys_4_NewKey.png)

"Look at that, Captain!  When we look back at our product licenses now, our note is there along with the new key."  Henry and his teammates felt proud for making it here.  They would not be defeated after all.  "You're almost there, Henry," said Captain vSAN.  "A few more steps, and we'll be finished."

![](17_ViewLicenseKeysAfterCombine.png)

Captain vSAN continued.  "Before we get too carried away, team, let's remove the 1 CPU licenses from vCenter.  You can do this from the same Assign License button we used previously."

![](18_RemoveLicenseKeysvCenter.png)

"Go ahead and confirm the removal of those excess keys.  Click Yes in the menu shown below."

![](19_RemoveLicensesConfirmation.png)

Henry knew what was coming.  "Now we're ready to add the new 4 CPU license key so we can assign the it to the cluster."

![](20_ImportNewCombinedLicense.png)

And this time, when they applied the license to the 4-node cluster, there were no errors.  The cluster was licensed, the mystery solved, and the implementation was complete...thanks to Captain vSAN, of course.

![](21_ClusterLicensedFeaturesFinal.png)

"Team, this is where I leave you," said Captain vSAN.  "The problems will come, but never stop looking for answers.  Seek to educate others, and remember the things I taught you.  Perhaps we will meet again someday."  Before anyone could thank him, Captain vSAN was gone just as quickly as he had arrived.

Minutes later, John walked back into the room.  No one realized he was nowhere to be found during all of the excitement until now.  "How's the project going, folks?"  The team let John know what Captain vSAN had done to help them solve the cluster license mystery and shared what they had learned.  "I can't believe I missed it," said John.  "Maybe I'll get to meet him some day."

After that the team always wondered...was John secretly Captain vSAN?  The world may never know.
