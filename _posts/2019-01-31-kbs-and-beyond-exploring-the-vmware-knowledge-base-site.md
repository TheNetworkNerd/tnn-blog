---
title: "KBs and Beyond - Exploring the VMware Knowledge Base Site"
date: 2019-01-31
media_subpath: /assets/images
permalink: /2019/01/31/kbs-and-beyond-exploring-the-vmware-knowledge-base-site/
categories: 
  - "VMware"
tags: 
  - "Documentation"
  - "vExpert"
  - "VMware"
  - "VMware Documentation"
image: "0_FeaturedImage.png"
---

I didn't realize when I wrote [this blog]({% link _posts/2018-11-30-the-extensive-guide-to-using-vmware-documentation-like-a-pro.md %}) on navigating VMware documentation that it would turn into a series, but it happened.  You can find part 2 of the series on customizing VMware documentation [here]({% link _posts/2018-12-07-tips-and-tricks-for-building-customized-vmware-docs.md %}).  In this installment (part 3), we will look at an adjacent area.  It's something that was mentioned in one of the previous articles, but we didn't give quite enough attention to it.  Are you ready for a treasure trove of knowledge?

Think of the last time you needed a knowledge base article (or KB for short) from a vendor.  Now narrow that specifically to VMware for a second.  It's likely you did a Google search before searching the KB site (maybe not every time but likely I imagine).  Fortunately for us, the web address for VMware KBs has become less messy.  They can be found to match the pattern https://kb.vmware.com/s/article/52084, for example, with 52084 being the KB number.  If you know the KB number, this makes life easy.  But how many people have them memorized?  I'd say not many do.

If you haven't visited it directly, the [VMware Knowledge Base site](https://kb.vmware.com/s/) has undergone a very nice face lift in recent years (similar to the look and feel of the [VMware Docs site](https://docs.vmware.com/)).  Wait a minute.  Why should you even care about that?  I showed you how to add a KB to a custom VMware doc in a [previous post]({% link _posts/2018-12-07-tips-and-tricks-for-building-customized-vmware-docs.md %}).  If we can just search for KBs from the main documentation site, why have a separate site for KB searches?  At first, I was thinking the same thing.  But I'll share with you what changed my mind.

 

### The VMware KB Site - First Glance

The first time you visit the [VMware KB site](https://kb.vmware.com/s/), you will likely be greeted with a welcome menu as shown in the below screenshot.  But if you aren't, click "See What's New" next to the search button to activate the welcome menu.

![](2_SeeWhatsNew.png)

At first glance, that list of items on the welcome menu won't blow you away.  It may not even move the needle enough to get you to enter a search term and use the site.  Wait - it has machine learning, and that means it's cool, right?  It certainly means I just saw an industry buzzword but at the moment nothing beyond that.  Stay with me.

![](1_WelcometoImprovedKnowledgeBase.png)

 

### A Search Begins

Once you click GOT IT to make the welcome menu disappear, try a search.  Let's try a search for workspace one and see what happens.

![](3_WS1Search.png)

 

As soon as we click search, notice how the scenery changes.  We now have some recent activity captured.  And we have a ton of results (as you would expect from a search so general).  From here, narrow the search by product, version, language, or date.  The default sort is by relevance instead of date.

![](4_PostSearchClick.png)

 

But if we look closely just before the start of the results list, there are two labels - "KB Articles (highlighted)" and "All Sources."  It's easy to see from the list of results returned that each item is clearly marked KB.  Am I the only one who feels like we've only scratched the surface here?  Have you been thinking about it too?  What happens if we click "All Sources?"

 

![](5_PostSearchClick_AllSources.png)

 

Now we see a Source menu in the left hand pane that allows us to search docs, community pages, blogs, or KB articles.  When I found this, I was really excited.  Let's narrow the sources to just communities and blogs and see what happens.

 

![](6_PostSearchClick_BlogsCommunities.png)

Every result shown now is either a blog or a [VMTN community](https://communities.vmware.com/community/vmtn/content) post containing our search terms.  Once specific sources are selected, we can no longer filter by product or version like in the previous screenshot, but filtering by language and date range are still options.  The "CLEAR" button in the upper right-hand corner will clear all selections in the left-hand pane.  That search was pretty general, so let's narrow the focus a bit.

 

### A Blog Search Easter Egg

If you recall from the post on [customizing VMware documentation]({% link _posts/2018-12-07-tips-and-tricks-for-building-customized-vmware-docs.md %}), searches of the [VMware Docs site](https://docs.vmware.com) will search KB articles (and much more).  Conversely, searches of the KB site will search product documentation.  There's some overlap in functionalities, but the two sites are quite different.  My favorite feature of the KB site is the ability to search blog posts.  VMware has a number of different blogs made by technical marketing and product team members that have helped me over the years.

To give an example, suppose we do a search of all blogs written this year for Skyline.  Here's what the results look like:

![](7_BlogSearchSkyline.png)

We have 10 results total, but let's examine the first 3 results together.  One article has Skyline in its title, while the other two have Skyline in the other data (a description) brought back by the search engine.

#### Result #1

The link doesn't take us to the blog post directly (screeenshot below).  It takes us to a specific [VMware Blog Beat](https://blogs.vmware.com/) feed item.  Click on the title of the article below to visit the blog post directly.  This blog entry is from the [VMware Support Insider blog](https://blogs.vmware.com/kb).

![](8_SkylineResult1.png)

#### Result #2

Yet again, the link takes us to an item in the blog feed mentioned earlier.  Click the article title to proceed to this entry from the [Partner News blog](https://blogs.vmware.com/partnernews).

![](9_SkylineResult2.png)

#### Result #3

I'm guessing you see the pattern now.  Once again we're directed to the blog feed housed in [Blog Beat](https://blogs.vmware.com) and can follow the link to get to the actual article.  But wait a minute....  This blog isn't a VMware curated blog at all!  This article was written on [Marc Huppert](https://twitter.com/MarcHuppert)'s blog but is promoting the blog article from our second result (the article from the [Partner New blog](https://blogs.vmware.com/partnernews) shown in the previous screenshot).

That seems a little odd, right?  Or does it?  If a blog search from the KB site is searching the [Blog Beat](https://blogs.vmware.com/) feed, that means I can search for keywords not only in official VMware blogs but also in blogs from some of the best virtualization minds in our industry.  This, to me, is amazing.  Take a deep breath, and we'll break it down further.  It gets better.

![](10_SkylineResult3.png)

 

### Further Analysis from Searching Blogs

If we go back to the search results from earlier (doing a search for "Skyline"), there are some entries that don't make any sense.  If you look at the results below (scroll down a bit to see this), none of the articles mention Skyline, but you can see the description in the results shows something about Skyline in each case.

![](11_SkylineResults_NoSense.png)

 

There is, of course, a reason for this behavior.  Suppose we follow that first link from the screenshot above and take a look (screenshot below).  You can see from the screenshot that this blog feed entry has links to what appear to be both the previous and next blog feed entry in chronological order.  It just so happens that a post entitled "Getting Started with VMware Skyline" is the next item in the feed.  The search we performed for Skyline searched blog titles, blog content, and other metadata on the blog entry (names of articles before and after).  That's a lot of noise we need to remove to find exactly the content we want.  When this happened to me the first time, I became significantly less ecstatic about my earlier discovery.  Don't quit on me now, though.

 

![](12_SkylineinNextPrevious.png)

 

Remember that welcome screen on the KB site?  There was a link shown that details [search prefixes and operators](https://kb.vmware.com/s/article/55692).  If you failed to read it, then you are in the same boat as me.  I thought the KB site was cool but had trouble getting what I truly wanted in my searches.  It's time, ladies and gentlemen, to level up from moderate skill level to the place where the elite search ninjas live.  Buckle your seatbelt.

I only want articles that specifically address Skyline, so searching for only those with Skyline in the title makes sense.  Look what happens if I change my search string to @title="Skyline" per this screenshot.

![](13_SkylineinTitle.png)

 

Only three results are returned.  Each one has Skyline in the title.  Yes!  Notice the article from Marc Huppert and the Partner News blog we saw previously were not returned.  Though the contents of each article mentions Skyline, they were excluded from the results because their titles do not contain the word Skyline.

 

### In Summary

- If you have not read [part 1]({% link _posts/2018-11-30-the-extensive-guide-to-using-vmware-documentation-like-a-pro.md %}) and [part 2]({% link _posts/2018-12-07-tips-and-tricks-for-building-customized-vmware-docs.md %}) in my documentation series, go back and give them a read.  It will give some better context to this post.

- Personally, I think the VMware KB site is really helpful.  It has some overlap in functionality with the [VMware Docs site](https://docs.vmware.com), but it also has some distinct differences.  The "All Sources" view is sweet like a hot fudge sundae, but the blog search functionality is the cherry on top.  I think being able to search for specific words in the title of a community blog is helpful, but I couldn't figure out a way to search blog titles and article bodies without bringing in the metadata for next and previous posts in the blog roll.  If someone out there figures it out, please let me know.

- If you have never looked at [Blog Beat](https://blogs.vmware.com), I highly recommend bookmarking the page.  Be sure to [register your blog](https://goo.gl/forms/2dkj0XUMpD4qyCaT2) with Blog Beat so people like me can search through its content easily.  You can also subscribe to the [RSS feed](https://blogs.vmware.com/wprss) as well.

- It was really interesting to me that the [VMware KB site](https://kb.vmware.com/s) has a special [advanced search features KB](https://blogs.vmware.com/wprss), but the [VMware Docs site](https://docs.vmware.com) has its own [Advanced Search page](https://docs.vmware.com/advanced-search.html).  But, from the main page of the Docs site, you will see that advanced search operators work.  I wasn't able to get that working with field names like on the KB site.  They may not be the same (not sure). ![](14_AdvancedSearch.png)
- It seems like at some point the Docs site and the KB site could be merged into one so there is only one place from which to search, but it's not my call.

- Remember how I mentioned machine learning earlier?  Take a look at the recommendations the KB site made after my searching and testing.  I primarily searched community pages and blogs. and those are the only types of results.  I'm guessing if my searches had been more general, you might see documentation page or KB recommendations here as well.![](15_RecommendedforYou.png)

 

### Further Reading

This blog is part 3 of a series on VMware documentation.  Check out the other posts in the series:

- Part 1 - [The Extensive Guide to Using VMware Documentation Like a Pro]({% link _posts/2018-11-30-the-extensive-guide-to-using-vmware-documentation-like-a-pro.md %})
- Part 2 - [Tips and Tricks for Building Customized VMware Docs]({% link _posts/2018-12-07-tips-and-tricks-for-building-customized-vmware-docs.md %})
- Part 3 - This post
- Part 4 - [Building Custom Search Engines for VMware Content in Google Chrome]({% link _posts/2019-02-27-building-custom-search-engines-for-vmware-content-in-google-chrome.md %})
- Part 5 - [Building Custom Search Engines for VMware Content in Mozilla Firefox]({% link _posts/2019-03-29-building-custom-search-engines-for-vmware-content-in-mozilla-firefox.md %})
