---
title: "A Tale of Two vCenters, VxRail Edition"
date: 2018-04-01
media_subpath: /assets/images
permalink: /2018/04/01/a-tale-of-two-vcenters-vxrail-edition/
categories: 
  - "VMware"
tags: 
  - "Hyper Convergence"
  - "vCenter"
  - "vCenter Deployment"
  - "VMware"
  - "VMware vCenter"
  - "VxRail"
  - "Deployment Planning"
  - "Decision Tradeoffs"
  - "Dell"
  - "Dell VxRail"
---

![](1_StartVxRailInstall.png)

Picture yourself in front of a rack of [VxRail](https://www.dellemc.com/en-us/converged-infrastructure/vxrail/index.htm) appliances.  Ah, the bright shine of freshly racked and cabled new gear!  The network team has VLANs ready.  The operations team has picked out naming conventions for host names and virtual machines.  Storage capacity will be more than enough to support growth efforts for the next year.  It's finally time to dive into the hyper-converged infrastructure pool of goodness.

Before jumping in, consider the tools used for cluster management in the VxRail world.  [VxRail Manager](http://rocknrails.com/what-is-vxrail-manager/) (a virtual machine within the cluster) is the secret sauce for lifecycle management, and vCenter is the tool for day-to-day operations.  But is deploying VxRail really as easy as stepping through the setup wizard and taking a lunch break?  It certainly can be, but don't let vCenter derail the success of your VxRail deployment.  This, my friends, is where our tale begins.

 

### Scenario 1

Suppose you're stepping through the VxRail 4.5 install wizard and reach this screen (part of the Networks step):

![](2_vCenterServerStep.png)

Based on the screenshot, you have selected the option to deploy a [vCenter Server Appliance](https://docs.vmware.com/en/VMware-vSphere/6.5/com.vmware.vsphere.vcsa.doc/GUID-223C2821-BD98-4C7A-936B-7DBE96291BA4.html) (VCSA) and [Platform Services Controller](https://www.vladan.fr/what-is-vmware-platform-service-controller/) (PSC) on the VxRail cluster.  This is referred to as the "VxRail vCenter Server" which comes licensed as part of your VxRail purchase.  Here are the pros and cons of this choice:

- Pros
    - The vCenter deployment is easy, fast, and done automatically.
    - When installing an update package for the VxRail cluster, both vCenter and the PSC get updated as part of this process.  That's lifecyle management at its finest.
    - Log Insight is deployed automatically when selecting this option. ![](3_LogInsight.png)

- Cons
    - In this deployment scenario, vCenter can only manage the VxRail cluster on which it is deployed.  Adding new VxRail nodes to scale out the cluster is supported, but managing other ESXi hosts or additional VxRail clusters is not.
    - The only vCenter topology supported is a single VCSA with external PSC.
    - [Enhanced linked mode](https://docs.vmware.com/en/VMware-vSphere/6.0/com.vmware.vsphere.install.doc/GUID-91EF7282-C45A-4E48-ADB0-5A4230A91FF2.html) is not supported.
    - [vSAN Encryption](https://blogs.vmware.com/virtualblocks/2017/04/11/vsan-6-6-native-data-at-rest-encryption/) is not supported.
    - The single sign-on (or SSO) domain must be vsphere.local (cannot be changed).
    - Migrating to an external (or "Customer Supplied") vCenter after initial deployment is **very difficult**  and was impossible to do in previous versions of VxRail without a factory reset.
    - Only one public ip address can be used for the [vCenter HA](https://docs.vmware.com/en/VMware-vSphere/6.5/com.vmware.vsphere.avail.doc/GUID-4A626993-A829-495C-9659-F64BA8B560BD.html) network.

 

### Scenario 2

Take a look at that screenshot again.  Do you see the check box labeled "Join existing vCenter Server?"

![](4_JoinExistingvCenter.png)

Selecting this option creates some requirements for the customer.  I'll list them here with the pros and cons of this decision:

- Requirements
    - A compatible vCenter Server must be fully deployed by the customer and available prior to VxRail setup.  Currently, VxRail 4.5 is compatible with [vCenter 6.5U1](https://my.vmware.com/web/vmware/details?productId=614&downloadGroup=VC65U1).

- Pros
    - This vCenter can be used to manage the entire VMware environment (VxRail clusters and any other ESXi hosts).
    - Enhanced linked mode, custom SSO domain name, and vSAN Encryption are supported.
    - Stretched clusters are supported.
    - All vCenter topologies are supported, even if you want to use a [Windows vCenter](https://blogs.vmware.com/vsphere/2017/08/farewell-vcenter-server-windows.html) (_shudder_).
    - Lifecycle management is still handled for everything but vCenter and Log Insight.

- Cons
    - The customer is responsible for licensing this instance of vCenter.
    - Both vCenter and the PSC cannot live on any VxRail cluster they are managing.  Ideally, vCenter should be part of a [management cluster](https://docs.vmware.com/en/VMware-Validated-Design/4.1/com.vmware.vvd.sddc-design.doc/GUID-AE659DBC-B1F0-4C0A-8F8D-555AA6649748.html).
    - All updates to vCenter and the PSC to versions compatible with VxRail are the responsibility of the customer and will not be handled by VxRail Manager.
    - Log Insight (should you choose to use it) must be deployed and maintained by the customer.
    - The virtual machine name and ip address for vCenter and the PSC cannot be modified after VxRail deployment.

 

### Choose Wisely

I suspect as time goes by, the two vCenter deployment scenarios will reach feature parity.  But, as of the writing of this post, there are some significant differences.

Which vCenter scenario is best for your VxRail deployment?  Should you spend more money for capabilities and flexibility but give up some lifecycle management?  Or, should you maximize the amount of lifecycle management and give up some capabilities?  Being educated about the ramifications of each scenario **before deployment** helps you get things right the first time based on your business requirements.

Don't just take my word for it.  Go read the VxRail vCenter Planning Guide.  Hopefully as the guide is updated, the link below will remain valid.

### Helpful Links

- [VxRail vCenter Planning Guide](https://www.emc.com/collateral/guide/vxrail-vcenter-server-planning-guide.pdf)
- [VxRail Spec Sheet](https://store.emc.com/en-us/Product-Family/VxRail-Products/Dell-EMC-VxRail-Appliance/p/VCE-VxRail?PID=EMC_SRS-VXRAIL-DC90_HERO)
- [VxRail Overview Deck](https://delltechnologiesworldonline.com/2017/connect/fileDownload/session/A2626A38D87A8E3D6A9D07CE818CB8ED/cpsd.11%20final.pdf)
