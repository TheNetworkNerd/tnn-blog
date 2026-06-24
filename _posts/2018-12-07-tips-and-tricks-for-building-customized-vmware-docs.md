---
title: "Tips and Tricks for Building Customized VMware Docs"
date: 2018-12-07
media_subpath: /assets/images
permalink: /2018/12/07/tips-and-tricks-for-building-customized-vmware-docs/
categories: 
  - "VMware"
tags: 
  - "Documentation"
  - "vExpert"
  - "VMware"
  - "VMware Documentation"
  - "Vendor Documentation"
  - "VMware Docs"
  - "Customized Documentation"
image: "manuscript-203465_640.jpg"
---

In [my last post]({% link _posts/2018-11-30-the-extensive-guide-to-using-vmware-documentation-like-a-pro.md %}), I gave advice on using VMware documentation like a pro.  But in this post, let's take that one step further to expert level.  There's something amazing about the VMware documentation site that people may not have explored.  From the [VMware Docs home page](https://docs.vmware.com), there's an option to build custom docs.  Let's click on it and see what happens.

![](1-BuildCustomDocs.png)

When you click the option above, you see a MyLibrary page briefly and then get redirected to a VMware Docs login page.  All you need is a [myvmware.com](https://my.vmware.com) account to login.  Clicking "Sign up for an account" will take you to the registration page for a myvmware.com account.

![](2-VMwareDocsLoginPage.png)

 

### Working with Collections

After logging in successfully, we arrive at the MyLibrary Collections page.  At this point there are no collections created, so let's give it a try.  Suppose we create a collection for vSAN documentation.  Name the collection whatever you like, and click create.

![](3_CreateCollection.png)

At this point, the collection has been created, but it contains nothing.  We can create additional collections or start adding to this one.

![](4-PostCollectionCreation.png)

I created one more collection just to see how it would look.  Notice the star next to the first collection we created.  This means "The vSAN Dream Collection" has been marked as the default collection.  We can change the default collection at any time, but for now, the default will remain as shown here.

![](5-TwoCollections.png)

Suppose we want to add the release notes for vSAN 6.7 U1 to a collection.  Since we are still logged in to the VMware Docs site, we can search for vSAN release notes to get a list of results and navigate to the appropriate documentation page.  The screenshot below shows the option to add this page (also called an item or topic) to MyLibrary.

![](6-AddtoMyLibrary.png)

If we click the option to "Add to MyLibrary" as shown above, the following message is displayed.  Notice this item was added to the default collection only.  We got no choice between collections.  It is best to know the default collection before adding items.

![](7-ItemAdded.png)

Let's fast forward a little.  At this point I have added three items to "The vSAN Dream Collection."  I went back to the MyLibrary page and selected "The Ultimate Cloud Collection" as the default (click on the star next to it), added 4 items to it, and set the default collection back to "The vSAN Dream Collection."  Here's how  MyLibrary looks after those changes:

![](8-Collectionwith7Topics.png)

Suppose we click the Manage link for "The vSAN Dream Collection."  We will see the 3 items in this collection listed in the left-hand pane.

![](9-vSANDreamCollectionwith3Topics.png)

Here are some navigational tips:

- Click on any item in the left-hand pane to read through the topic.
- Drag and drop items in the left-hand pane to re-order or make one a subtopic of another.
- Remove items from the collection in two ways:
    - Remove a single item by highlighting the topic in the left-hand pane and clicking "Remove From MyLibrary" in the right-hand pane.
    - Remove multiple items by selecting them in the left-hand pane and clicking the remove button at the top of the left-hand pane (which turns red once you select at least one item).   
    ![](10-RemoveItem.png)
- If you have multiple collections that contain topics, you can switch between the collections by clicking the collection name in the left-hand pane.
- To delete an entire collection, go back to the MyLibrary page, and click the delete option next to the collection's name.
- There was no way (that I could find) to move a topic from one collection to another.  This is why I would recommend checking the default collection before adding anything to MyLibrary.

 

### Sharing is Caring

Suppose we have created several collections. At this point, all collections are accessible only through one specific myvmware.com account.  What if those collections would be helpful to a colleague or industry peer?  Suppose "The vSAN Dream Collection" contains so much goodness it would be folly not to share it with the world.  Sharing is done on a per-collection basis and can be activated from either the MyLibrary main page or from the collection management area.

**Option 1 - Share from MyLibrary Collections Page**

![](11-SharefromCollections.png)

**Option 2 - Share from within a Collection**

![](12-ShareButtonCollection.png)

 

Once we click Share using option 1 or 2 above, the following screen pops into view.  We see a link provided below, but since sharing has not yet been activated, the link does not work.

![](13-ClickShare2.png)

Once sharing is activated as seen here, the link is live.  The entire collection has been shared (no way to share a subset of a collection).  We can copy the link or share on social media if the mood suits us.  The only option for securing access to the link is "anyone with the link can view."

![](14-ClickShare2.png)

Now that the link is live, let's see what following it actually does.  We see below all three topics that were added to the collection but in a different order from the previous screenshot.  When the owner of the collection puts the topics in a different order or adds / removes items, anyone who has the shared link will see the changes.  Note that clicking on specific topics in the shared collection changes your browser address, so revisiting the original shared link might be needed to see changes to the collection (and to prevent 404 errors) if items were moved around or removed.  The link to a collection is read-only.  Only the collection owner can make changes to a collection's contents.

![](15-LinkFollowed.png)

 

### Extending Collections to Include KB Articles

Everything we've added to a collection thus far has been a specific item / topic from the VMware Docs site.  In order to add a topic to a collection, we had to first visit the topic page and click the option to "Add to MyLibrary."  But when you search the VMware Docs site, some of the results are links to other areas, like knowledge base (or KB) articles.

Suppose we search for the best KB article ever created, "What You Can and Cannot Change in a vSAN ReadyNode."  In the first result below, you see the name of the KB and an icon in the far right corner which indicates this is a link to an external site.  But also notice we have the option to "Add to MyLibrary" without actually following the link to the KB.

![](16-SearchingforaKB.png)

If we add the KB to MyLibrary (i.e. adding it to "The vSAN Dream Collection"), here's what the collection looks like to someone with the shared link.  Notice the full contents of the KB were not pulled in (just a description and link to it), but as the collection owner, I've just bookmarked a KB in my collection.  If there were several KBs I frequented, I could create a collection to keep it all in one spot and later share with the other members of my team.

![](17-KBinCollection.png)

Why stop now?  KBs are not the only form of external content we can add to a collection.  If we search the VMware Docs site for vSAN, look at the different content sources it is searching.  Anything that is not "Product Documentation" will be an external link.  Videos, anyone?

![](18-ContentSources.png)

### In Summary

I'm leaving the link to "The vSAN Dream Collection" live for anyone to view.  Check it out [here](https://docs.vmware.com/share/2df6f06d18a494b11bf6717296325999).  I hope to add more content to the collection as I find it (or as I have time) since there are only a few things there now.

Overall I think the capability to build custom documents (i.e. create and share collections) is amazing.  I hope we will see more functionalities added in time.  Here are my recommendations for some changes that would be very helpful to users of the site:

- There is no way to rename a collection after it is created that I could find.  I can see the need to do this.
- Once a collection is shared, there is no indicator on the MyLibrary page or on the collection's page itself to remind you that the collection has been shared.  The only way you would know is by clicking the "Share" button.
- There was no way to move topics / items from one collection to another.  I can see this being very useful as well as the ability to copy items to another collection.
- When adding to MyLibrary, anything you add goes into the default collection.  I'd love to see a workflow that allows you to choose the collection in which a topic / item gets placed just in case you forgot to check the default collection setting after logging in.
- I can see some role-based access being very helpful on this site.  At the moment, if the owner of a collection left the company, the collection is tied to the person's myvmware.com account and would remain read-only.  If the myvmware.com roles and permissions could be changed to allow editing collections created by any user at the company with a myvmware.com account (or even just the superuser), that would be a game changer.  I can see not wanting to allow editing of a collection that is shared with folks outside of an organization.
- If a search of the Docs site searched through all VMware blogs as well, these would be very helpful to add to a collection.  I find myself sending out links to good VMware blogs pretty frequently.
- The one-click add to MyLibrary does not handle or advise against duplicate additions, which would be problematic with large custom libraries.
    - Full credit to [Eric Allione](https://twitter.com/vNetworking) for finding this one!
- It would be nice to add links to the compatibility guide to a collection.
    - Full credit to [John White](https://twitter.com/vjourneyman) for this suggestion!

 

### Further Reading

This blog is part 2 of a series on VMware documentation.  Check out the other posts in the series:

- Part 1 - [The Extensive Guide to Using VMware Documentation Like a Pro]({% link _posts/2018-11-30-the-extensive-guide-to-using-vmware-documentation-like-a-pro.md %})
- Part 2 - This post
- Part 3 - [KBs and Beyond – Exploring the VMware Knowledge Base Site]({% link _posts/2019-01-31-kbs-and-beyond-exploring-the-vmware-knowledge-base-site.md %})
- Part 4 - [Building Custom Search Engines for VMware Content in Google Chrome]({% link _posts/2019-02-27-building-custom-search-engines-for-vmware-content-in-google-chrome.md %})
- Part 5 - [Building Custom Search Engines for VMware Content in Mozilla Firefox]({% link _posts/2019-03-29-building-custom-search-engines-for-vmware-content-in-mozilla-firefox.md %})
