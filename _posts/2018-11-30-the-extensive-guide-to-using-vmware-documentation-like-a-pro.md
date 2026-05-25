---
title: "The Extensive Guide to Using VMware Documentation Like a Pro"
date: 2018-11-30
media_subpath: /assets/images
permalink: /2018/11/30/the-extensive-guide-to-using-vmware-documentation-like-a-pro/
categories: 
  - "VMware"
tags: 
  - "Documentation"
  - "vExpert"
  - "VMware"
  - "VMware Documentation"
image: "VMware-Docs.png"
---

When a software company has a large number of products, it can be difficult to find exactly what you need.  So how do you navigate the sea of KBs, blogs, documentation, and other helpful items when it comes to VMware stuff?  At first, it seems overwhelming.  But if you know where to look, the answers are available.  I'll start with some of the links I use most often and branch out from there, telling you where to look and what you will find when you do.  And here's the best part.  All of this is completely free.

 

### A General Tool Set

- [Correlating build numbers and versions of VMware products](https://kb.vmware.com/s/article/1014508)
    - Build numbers just don't correlate to version numbers well, but there is hope.  This KB contains correlation links for multiple products, including VxRail and Cloud Foundation.

- [VMware Compatibility Guide](https://www.vmware.com/resources/compatibility/search.php)
    - It's not just about checking to see if a server and its processor are on the compatibility list.  Check NIC cards and firmware versions, supported BIOS levels, storage controllers, storage devices and their firmware, graphics cards, VVOLs support, and much more.  The "what are you looking for" box has many options.  Take advantage.![](1-WhatAreYoulookingFor.png)
    - Next to the box above you will see the words "Compatibility Guides." ![](2_CompatibilityGuides.png)
    - If you click the dropdown arrow next to those words, something absolutely magical happens. ![](3-CompatibilityGuidesExpand.png)
    - Have you ever wanted to see what new items were added to the compatibility guide or download a copy for reference?  Alas, now you can.  Anything in the menu above is clickable and opens in a new browser window with an address bar.

- [Product Interoperability Matrix](https://partnerweb.vmware.com/comp_guide2/sim/interop_matrix.php#interop&2=&1=)
    - Are the various products in my environment and their respective versions compatible with one another?  Find out here.

- [Product Interoperability Matrix – Upgrade Path Tab](https://partnerweb.vmware.com/comp_guide2/sim/interop_matrix.php#upgrade&solution=1)
    - This seems to be a lesser known trick.  Use this tab to determine how far you can upgrade based on your current version of a product.

- Upgrade Sequence for Multiple Products
    - It is critical products be upgraded in the proper order for functionality and supportability.
    - [Update sequence for vSphere 6.7 and its compatible VMware products](https://kb.vmware.com/s/article/53710)
    - [Update sequence for vSphere 6.5 and its compatible VMware products](https://kb.vmware.com/s/article/2147289)
    - [Update sequence for vSphere 6.0 and its compatible VMware products](https://kb.vmware.com/s/article/2109760)

- VMware Support and Product Lifecycle
    - [Product Lifecycle Policies](https://www.vmware.com/support/policies/lifecycle.html)
        - What do things like General Support Phase and Technical Guidance mean in terms of product support?  Get all of that information here.
    - [Lifecycle Product Matrix](https://www.vmware.com/content/dam/digitalmarketing/vmware/en/pdf/support/product-lifecycle-matrix.pdf)
        - When is the end of general support / technical guidance for a specific product?
    - [Support Offerings](https://www.vmware.com/support/services.html)
        - What are the VMware support offerings, and what is the different between them?
    - [Product Support Matrix](https://www.vmware.com/content/dam/digitalmarketing/vmware/en/pdf/support/support-by-product-matrix.pdf)
        - What support offerings are available for each product in the portfolio?

- [VMware Ports and Protocols](https://ports.vmware.com/home)
    - Have you ever wondered what ports a specific product uses?  What if you could search by product, port, protocol, export the results, and experience it all in dark mode? ![](VMwarePortsandProtocols-1024x460.png)

- [Config Max Tool](https://configmax.vmware.com/home)
    - Remember the configuration maximums that used to live in large PDF documents that we studied for VCP exams?  Now they exist in an online tool for your consumption.  This site is more than meets the eye.
        - **Pro Tip:**  Sure, you can view configuration maximums for a specific product version, but you can also _compare_ the maximums between different versions of products to see if anything changed.  The screenshot below shows a comparison of some of the maximums for the extra small deployment of vROps across 3 different versions (all exportable to CSV). ![](VMwareConfigMaximums_4-1024x643.png)
- [VMware Product Guide](https://www.vmware.com/content/dam/digitalmarketing/vmware/en/pdf/product/vmware-product-guide.pdf)
    - This is the consistently updated guide to license terms and conditions for on-premises products.
    - As a compliment to this guide, the VMware EULA for on-premises products and Terms of Service for cloud and hosted services can be found [here](http://www.vmware.com/download/eula).

 

### Security

- [VMware Security Advisories](https://www.vmware.com/security/advisories.html)
    - If you like to stay aware of any and all security advisories, this is the place for you.  Sign up to receive an e-mail when new security advisories are posted by the [VMware Security Response Center](https://www.vmware.com/security/vsrc.html).
    - Security configuration guides (formerly hardening guides) can be found [here](https://www.vmware.com/security/hardening-guides.html).
- [VMware Security Certifications](https://www.vmware.com/security/certifications.html)
    - What products and modules have been validated against FIPS 140-2?  Find out here.
- [Security Development Lifecycle](https://www.vmware.com/security/sdl.html)
    - Learn more about our methodology for developing secure software.

 

### Product Documentation Repositories

It seems like product documentation is scattered among several sites.  Here are the major hubs:

- [StorageHub](https://storagehub.vmware.com)
    - This is the site for all things related to VMware storage and availability.  It's a massive collection of information on vSAN, SRM, and vSphere Storage (VVOLs, VMFS, core storage, etc.).
    - Make yourself smarter with planning guides, sizing guides and calculators, evaluation guides, technical overviews, best practice guides, design guides, and FAQs.
    - You can even find helpful podcasts on [vVols](https://storagehub.vmware.com/t/vsphere-storage/vvols-podcasts) and [vSAN](https://storagehub.vmware.com/t/vmware-vsan/vsan-podcasts/).

- [Product Walkthroughs](https://featurewalkthrough.vmware.com)
    - It's one thing to read through the installation / administration guide for a product, but what if you could see a click-by-click shot of the interface to help you get started?  This is the place for it.
    - **Warning:** Not all content on this site is completely up to date, but it's helpful to understand some of the steps required for specific tasks.

- [vRealize Suite Site](https://vrealize.vmware.com)
    - This site contains a wealth of information about the [vRealize Suite](https://www.vmware.com/products/vrealize-suite.html).  Check out how-to videos, guided tours, product walkthroughs, and more!
    - Don't forget to browse the [vRealize Operations Dashboard Sample Exchange](https://vrealize.vmware.com/sample-exchange/) while you're visiting.
    - **Pro Tip:** Type _tags: vROps 7.0_ in the search box for a list of all content specific to vRealize Operations 7.0.

- [Digital Workspace Tech Zone](https://techzone.vmware.com)
    - This is the site for all things under the end user computing umbrella and easily one of the coolest sites we have.  That's everything related to Workspace ONE and Horizon.  Browse through the plethora of reference architectures, technical overviews, tutorials, videos, technical tools, and blogs to master all things under the EUC umbrella.  New content is consistently being added to this site.  Be sure to login to the site with your myvmware.com account to rate and pin content! ![](TechZone_6-1024x409.png)
    - **Pro Tip:** Take advantage of the customized activity paths for [Horizon](https://techzone.vmware.com/becoming-horizon-hero-0) or [Workspace ONE](https://techzone.vmware.com/becoming-workspace-one-hero) to help you become a hero in one or both areas.  Start with the beginner content, and proceed to intermediate and advanced when ready. ![](4_ActivityPaths.png)

- [Networking and Security Tech Zone](https://nsx.techzone.vmware.com/)
    - Similar to Digital Workspace Tech Zone, this is the place to find blogs, videos, demos, and guides in the networking and security space.  Educate yourself on AppDefense, NSX, VeloCloud, HCX, vRNI, or NSX Cloud.
    - In addition to some very granular searchability (as shown below), there are customized learning paths here as well. ![](NetSecurityTechZone_3-1024x342.png)

- [VeloCloud](https://www.velocloud.com/sd-wan-resources)
    - In addition to the content in Networking and Security Tech Zone, check out a number of solution overviews, case studies, eBooks, webinars, and podcasts specific to VeloCloud and SD-WAN.

- [vSphere Integrated Containers](https://vmware.github.io/vic-product/#overview)
    - This is the place for release notes and documentation for vSphere Integrated Containers (VIC).
    - The official VIC Github site can be [here](https://github.com/vmware/vic-product).

- [VMware Cloud Products](https://cloud.vmware.com)
    - This site doesn't just cover [VMware Cloud on AWS](https://cloud.vmware.com/vmc-aws), but it's probably the most well known.  This product in particular has an online [FAQ](https://cloud.vmware.com/vmc-aws/faq) as well as a public-facing [roadmap](https://cloud.vmware.com/vmc-aws/roadmap).
    - There are many more products in the cloud portfolio like Log Insight Cloud, HCX, and Wavefront.  Most of them have a FAQ page and a Resources page anyone can leverage to learn more about the product. ![](5_LogIntelligence.png)

- [Wavefront](https://docs.wavefront.com/)
    - Interestingly enough, Wavefront documentation has its own repository.  The documentation includes helpful videos, information on integrations, and a query language reference.

- [VMware Cloud Marketplace](https://marketplace.cloud.vmware.com/)
    - Leverage this site to search for third-party solutions and services, designed and tested to run on VMware-based clouds.  Each item in the catalog has links to additional resources, prerequisites, and information on support.
    - Read about the initial availability of Cloud Marketplace [here](https://cloud.vmware.com/community/2019/08/26/marketplace-initial-availability/).

- [vSphere Central](https://vspherecentral.vmware.com)
    - This is the place to find walkthroughs, demos, and highlights of new features in vSphere.
    - **Pro Tip:** Check out the vSphere Upgrade section to help plan your next upgrade.  And don't forget [Nigel Hickey's vSphere upgrade blog series](https://blogs.vmware.com/vsphere/2018/12/the-vsphere-upgrade-blog-series-wrap-up.html). ![](6-vSphere6.7.png)

- [My Workspace ONE Support Site](https://support.workspaceone.com/)
    - This site has its own sets of documentation for Workspace ONE as well as [knowledge base articles](https://support.workspaceone.com/kb?sort=newest), [webinars](https://resources.workspaceone.com/marketing/webinars?sort=newest&version=1811), and [community forums](https://support.workspaceone.com/forums?sort=newest).
    - **Pro Tip:** Subscribe to the [weekly KB digest](https://support.workspaceone.com/kb/weekly-knowledge-base-digest), [new product announcements](https://support.workspaceone.com/kb/product-announcements), or [articles on known issues](https://support.workspaceone.com/kb/known-issues?sort=newest&version=1811).

- [VMware Solution Exchange](https://marketplace.vmware.com/vsx/)
    - This is a place to find solutions that complement, integrate, or interoperate with VMware solutions.  Each solution has technical specifications, links additional documentation, community reviews, and information about support.
    - Examples of what you might find here include the following:
        - Third party management packs for vROps / vRA
        - Endpoint devices (thin clients) and [peripherals](https://marketplace.vmware.com/vsx/content/vmware-validated-peripherals) for use with Horizon
        - Software that integrates with NSX

 

### Documentation and KB Sites

- [Official VMware Documentation Site](https://docs.vmware.com)
    - This is the place to visit for general product documentation (including release notes, installation and administration guides, term definitions, etc.).  Get it in HTML or PDF format depending on your preferred consumption model.
    - Browse all products covered in the documentation [here](https://docs.vmware.com/allproducts.html).
        - **Pro Tip:**  This includes documentation on [VMware Validated Design](https://docs.vmware.com/en/VMware-Validated-Design/index.html) as well as [VMware Skyline](https://www.vmware.com/support/services/skyline.html) (proactive support technology that is free to customers covered under Production Support or higher).
        - **Expert Tip:**  Learn how to build customized VMware docs by reading [this post]({% link _posts/2018-12-07-tips-and-tricks-for-building-customized-vmware-docs.md %}).

- [Official VMware Knowledge Base Site](https://kb.vmware.com/s/)
    - Search this site for KBs based on product name, version, and even based on a specific date range.
    - **Expert Tip:**  Find more than just KBs on this site.  Learn about all the different things you can find using the KB site by reading [this post]({% link _posts/2019-01-31-kbs-and-beyond-exploring-the-vmware-knowledge-base-site.md %}).

 

### Blogs

Another great source of helpful documentation is in blog form.

- Did you know there is an aggregation of VMware blogs?  The screenshot below does not do it justice, so check out [https://blogs.vmware.com/all-vmware-blogs](https://blogs.vmware.com/all-vmware-blogs) to see what you've been missing.  If you're a Chrome user, there is a [RSS Feed Reader](https://chrome.google.com/webstore/detail/rss-feed-reader/pnjaodmkngahhkoihejjehlcdlnohgmp?hl=en) which could come in handy for this. ![](7-AllVMwareBlogs.png)

 

- While I cannot guarantee the list of blogs above will be completely comprehensive, I do want to point out some which people may not know exist:
    - [VMware Cloud Provider Blog](https://blogs.vmware.com/vcloud/)
        - Stay on top of the current news in the world of VMware service providers.
    - [VMware Support Insider](https://blogs.vmware.com/kb/)
        - Keep track of the latest set of KBs released, and subscribe to the ones that are most important.  Learn about new enhancements to VMware Skyline and other news.
    - [VMware Careers](https://blogs.vmware.com/careers/)
        - Did you know we had a careers blog?  Maybe this can give candidates some idea of what it's like to be a part of VMware.
    - [VMware Education](https://blogs.vmware.com/education/)
        - Are you paying attention to the certification programs and how they have changed or what new courses might be offered?  This is the place.
    - [VMware Open Source Blog](https://blogs.vmware.com/opensource)
        - What is VMware up to in the open source world?  What is it like to work in the VMware Open Source Technology Center?  Get the answers to these questions and more!
    - [VMware Cloud Advocacy Team Blog](https://www.cloudjourney.io/)
        - If you're trying to develop applications or operate in native public clouds and want to see examples of best practices, this is the place.
    - [VMware Design Team Blog](https://medium.com/vmwaredesign)
        - Keep tabs on the team that designs user experience at VMware.
    - [VMware Security and Compliance Blog](http://blogs.vmware.com/security/)
    - [VMware Cloud Native Apps Blog](https://blogs.vmware.com/cloudnative/)
        - Read when you want or subscribe.

 

### Community

- [VMware {code}](https://code.vmware.com/resources)
    - If you're a script writer, developer, or looking to learn more about the wonderful world of automation, check out the resources available on the VMware {code} page.  You can find SDKs, developer forums, design standards, sample code, and a number of developer and automation tools.
    - **Pro Tip:**  [Join the VMware {code} community](https://code.vmware.com/join) and participate in the Slack channel with other experts.
    - **Expert Tip:** Participate in the [VMware {code} Speaker Bureau](https://code.vmware.com/web/code/speaker-bureau) to polish your presentation skills and create content to help others.

- [Project Clarity](https://clarity.design/)
    - Project Clarity is an open source design system that brings together UX guidelines, an HTML/CSS framework, and Angular components.
        - Used by over 100 VMware products!
    - [Clarity on GitHub](https://github.com/vmware/clarity/)

- [VMTN](https://communities.vmware.com/community/vmtn/content)
    - If you're not a member of the VMware Technology Network, it can be a great (and free) resource to crowd source answers to problems.
    - **Pro Tip:** Take some time to browse the [forums](https://communities.vmware.com/community/vmtn/overview).  Most forums have a Documents section which may contain very valuable information (e.g. the documents for [User Environment Manger](https://communities.vmware.com/community/vmtn/user-environment-manager/content?filterID=contentstatus[published]~objecttype~objecttype[document]), the [NSX-T Reference Design](https://communities.vmware.com/docs/DOC-37591), etc.).
        - A collection of documents from all forums can be found [here](https://communities.vmware.com/docs) as well.

- [VMware Cloud Community](https://cloud.vmware.com/community/)
    - This community is separate and apart from VMTN.  Take the time to check it out, join the Slack, and sign up for the newsletter.

- [VMware Design](http://vmware.design/research.html)
    - Follow the link above to join the VMware Design Studio Program and participate in product feedback studies.

- [VMware Flings](https://labs.vmware.com/flings)
    - Flings are apps and tools built by VMware engineers or other community members that are free to use.  Use of one requires you to read and agree to a Technical Preview License and is not officially supported by VMware.  However, a number of flings have made their way into VMware products (ESXi Embedded Host Client, VCS to VCSA Converter, etc.).
    - If nothing else, [subscribe to the quarterly newsletter](https://labs.vmware.com/flings/signup) to stay on top of what's new.

 

### Learning Platforms, Etc.

- [VMware Learning Zone](https://www.vmware.com/education-services/learning-zone.html)
    - This isn't really a collection of documentation, but it is a collection of eLearning courses and instructional videos.  The Basic Learning subscription is free for 1 year.  It's worth a look.
    - Stay on top of new additions to the VMware Learning Zone [here](https://mylearn.vmware.com/mgrReg/plan.cfm?plan=100479&ui=www_edu).

- [VMware Hands-on Labs](https://labs.hol.vmware.com/HOL/catalogs/catalog/all)
    - If you think there is no documentation inside the hands-on labs, you are mistaken!  Lab manuals are written to explain new concepts and guide you through how to perform actions inside the right user interface.  Read these manuals carefully for some hidden gems of wisdom.

- [TestDrive](https://www.vmwdemo.com)
    - This is a step beyond hands-on labs.  Imagine being able to take a device and enroll it with Workspace ONE to see how applied policies affect the device and user experience.  For more on other things you can do in TestDrive, check out [this page](https://kb.vmtestdrive.com/hc/en-us).
    - This site is free for VMUG Advantage members.  You can still sign up for temporary free access [here](https://www.vmtestdrive.com/signup).

- [Kubernetes Academy](https://kubernetes.academy/)
    - While this is not product specific documentation, this site can teach you many things about Kubernetes and containers.

- [VMworld On-Demand Video Library](https://videos.vmworld.com)
    - Thousands of hours of presentations are captured and free for consumption.  Most have an accompanying PDF of the slide deck for reference.  All you need is a free MyVMworld account to login.

- VMware Podcasts
    - Listen to technical podcasts on the products before / while / after diving into the documentation.
    - [VMware Communities Roundtable Podcast](https://www.talkshoe.com/show/vmware-communities-roundtable)
    - [Virtually Speaking Podcast](https://www.vspeakingpodcast.com/)

### Miscellaneous

- Check out [Tim Sandy](https://twitter.com/VMTimSandy)'s incredible compilation of [useful VMware links](https://twitter.com/VMTimSandy).

 

### Summary

While this is meant to be as exhaustive as possible, I would love to know if I have missed something so I can add it for the benefit of others.  I hope this article helps you solve a problem quicker because you knew where to look.

Also, if you're looking for the Powerpoint version of this blog that was delivered at the Boston VMUG UserCon in September 2019, you can find it on [my GitHub](https://github.com/TheNetworkNerd/Presentations/tree/master/The%20Extensive%20Guide%20to%20Using%20VMware%20Documentation%20Like%20a%20Pro).

 

### Further Reading

This blog is part 1 of a series on VMware documentation.  Check out the other posts in the series:

- Part 1 - This Post
- Part 2 - [Tips and Tricks for Building Customized VMware Docs]({% link _posts/2018-12-07-tips-and-tricks-for-building-customized-vmware-docs.md %})
- Part 3 - [KBs and Beyond – Exploring the VMware Knowledge Base Site]({% link _posts/2019-01-31-kbs-and-beyond-exploring-the-vmware-knowledge-base-site.md %})
- Part 4 - [Building Custom Search Engines for VMware Content in Google Chrome]({% link _posts/2019-02-27-building-custom-search-engines-for-vmware-content-in-google-chrome.md %})
- Part 5 - [Building Custom Search Engines for VMware Content in Mozilla Firefox]({% link _posts/2019-03-29-building-custom-search-engines-for-vmware-content-in-mozilla-firefox.md %})
