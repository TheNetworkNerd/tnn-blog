---
title: "Building Custom Search Engines for VMware Content in Google Chrome"
date: 2019-02-27
media_subpath: /assets/images
permalink: /2019/02/27/building-custom-search-engines-for-vmware-content-in-google-chrome/
categories: 
  - "VMware"
tags: 
  - "Custom Search Engines"
  - "Documentation"
  - "Google Chrome"
  - "Chrome"
  - "Search Engines"
  - "vExpert"
  - "VMware"
  - "VMware Documentation"
  - "Browser Customization"
image: "google-76517_640.png"
---

In [my previous post in the VMware documentation series]({% link _posts/2019-01-31-kbs-and-beyond-exploring-the-vmware-knowledge-base-site.md %}), I explored the VMware knowledge base site and some of the unexpected functionalities it brings if you pay close attention.  I would encourage going back to read that post before continuing here as I believe it will give you a better foundation for what we're about to do.

To begin, visit the [VMware Knowlegde Base site](https://kb.vmware.com/s/), and click on "See What's New" next to the search button.  The screen you see should be a familiar one.

![](1_WhatsNew.jpg)

But I want to explore something else this time.  Click the option to learn how to add a search shortcut on Google Chrome.  As you will see, the article is helpful and not very helpful at the same time.  It will be helpful if you didn't know how to add a search engine to Google Chrome.  Before we go adding a search engine to Chrome, let's do a brief search of the VMware Knowledge Base site to see if that helps with the setup.  Try a search for vSAN disk groups, and see what happens.  But don't look at the page results.  Look at the address bar in Chrome.  Here's how it would look using the search term I mentioned:
```
https://kb.vmware.com/s/global-search/%40uri#q=vSAN%20disk%20groups&t=Knowledge&sort=relevancy
```

Try searching for something else.  How about we try Horizon connection server this time and see what happens?  Here's the Chrome address bar as a result:
```
https://kb.vmware.com/s/global-search/%40uri#q=Horizon%20connection%20server&t=Knowledge&sort=relevancy
```

Those are similar enough to help us establish a pattern.  Stay with me as we continue.
 

### Adding a Google Chrome Custom Search Engine

I'll assume you read the article on how to add a search engine to Chrome and have made it to the Manage Search Engines area.  Now click Add.  Here's what you will see.

![](2_ChromeAddNewBlank.jpg)

We're presented with 3 fields at this point.  We're going to fill them out in a very specific way so that we can use a keyword in Chrome to search the VMware Knowledge Base site for whatever term(s) we want.  Use the parameters below.  I am pasting them in plain text here so you can copy them and speed up the process.  The keyword field can be anything you like.  I chose vmkb for the purposes of this exercise.  The last field was the most challenging to get right, and it's part of the reason I showed two different examples above to reveal the pattern.  Notice the placement of the **%s** below compared to the keywords our searches above.  Remember also that **%20** in the browser address bar replaces spaces in search terms.  See [this link](https://www.w3schools.com/tags/ref_urlencode.asp) for more information on that.

**Search engine:** https://kb.vmware.com/s

**Keyword:** vmkb

**URL with %s in place of query:** 
```
https://kb.vmware.com/s/global-search/%40uri#q=%s&t=Knowledge&sort=relevancy
```

And if you prefer a screenshot, here it is.

![](3_ChromeAddNewFilled.jpg)

Be sure to click Save and verify you can see the newly added search engine in the Chrome search engine list.

![](4_SearchEngineEntry.jpg)

 

### Further Experimentation

The search engine is now ready, so let's take it for a spin.  Open up a new Chrome tab, type the keyword vmkb, and hit the space bar to see something magical.  This magic trick will work for the keyword you chose previously.

![](5_SearchVMwareKB.jpg)

At this point, put in your search term(s).  To ensure we got the parameters correct, let's use vSAN disk groups as our search terms again.

![](6_SearchwithSearchTerms.jpg)

Press enter to perform the search, and you will be whisked away to the VMware Knowledge Base site and will notice a search was done for vSAN disk groups (just like you commanded).  Our browser address bar matches exactly what we saw when the search was performed manually on the Knowledge Base site, so that means we have done things correctly to this point.

That's fine as a baby step, but we're not going to stop there.  If you recall from [my previous post]({% link _posts/2019-01-31-kbs-and-beyond-exploring-the-vmware-knowledge-base-site.md %}), the default search on the VMware Knowledge Base site searches KB articles only.  But if we click the All Sources option after performing a search, we can search VMware documentation, community pages, KB articles, and blogs from [VMware Blog Beat](https://blogs.vmware.com/).  Look at how the browser address bar changes for the same search terms (vSAN disk groups) if we click All Sources:
```
https://kb.vmware.com/s/global-search/%40uri#q=vSAN%20disk%20groups**&t=MoreContent&sort=relevancy**
```

If we wanted all searches that leverage the search engine in Chrome to search this way, it would require modifying our existing search engine accordingly.  Before we do that, let's try some additional experimentation.  We can tweak the KB site to let us search for blogs, remember?  If we search for vSAN disk groups once more but choose the source to be only blogs, here's what the address bar looks like:
```
https://kb.vmware.com/s/global-search/%40uri#q=vSAN%20disk%20groups**&t=MoreContent&sort=relevancy&f:@commonsource=\[Blogs\]**
```

What if we want to pull back results with vSAN disk groups in the title?  First, make sure you read the [prefixes and operators KB](https://kb.vmware.com/s/article/55692) for some ninja tricks.  Then execute the search manually from the VMware Knowledge Base site as in the screenshot below.

![](7_SearchwithTitle.png)

That changes the address bar once more to the following:
```
https://kb.vmware.com/s/global-search/%40uri#q=**%40title%3D%22vSAN%20disk%20groups%22&t=MoreContent&sort=relevancy&f:@commonsource=\[Blogs\]**
```

Notice here that the operator (@title and the opening and closing quotation marks) got encoded as part of the browser address.  That's fine.  When searching, it's advantageous to use the operators and prefixes as you like without hard coding them into your custom search engine in Chrome.  This begs the question...what is the best way to setup my custom search engine in Chrome to get the most value?

Consider the areas you're visiting most often from a general Google search.  To me, the ability to search [VMware Blog Beat](https://blogs.vmware.com/) is absolute gold, so I will likely want to search for blogs no matter what.  I find official VMware docs useful as well, so I'd want that repository searched too.  While I enjoy a good KB article, I am going to leave it out of the results for now.   Again, this is totally subjective.  If we perform the same search as in the screenshot above (@title=#vSAN disk groups") and select only Docs and Blog as the sources, here's what the address bar shows:
```
https://kb.vmware.com/s/global-search/%40uri#q=%40title%3D%22vSAN%20disk%20groups%22&t=MoreContent&sort=relevancy**&f:@commonsource=\[Docs,Blogs\]**
```
 

### Tweaking the Google Search Engine

Go back to the custom search engine we created in Chrome, and it is time to make some adjustments.  Here are the parameters we will need to do a search of Docs and Blogs only.  Let's go ahead and change the keyword from vmkb to vmdoc as well.

**Search engine:** https://kb.vmware.com/s

**Keyword:** vmdoc

**URL with %s in place of query:** 
```
https://kb.vmware.com/s/global-search/%40uri#q=%s&t=MoreContent&sort=relevancy&f:@commonsource=\[Docs,Blogs\]
```

And if you prefer a screenshot, here it is.

![](8_AfterSearchEngineChanges.png)

Now give it a shot.  Test it by typing vmdoc in a new Chrome tab, hitting the space bar, and then typing @title="vSAN disk groups."  Press enter to conduct your search.  Does the result set match the same as a manual search of the KB site with the same search terms and results narrowed to Docs and Blogs only?  They should.

 

### A Search Engine for the VMware Docs Site?

I figured if we could make a custom search engine using the VMware Knowledge Base site, why not try the [VMware Docs site](https://docs.vmware.com/) also?  On the Docs site home page specifically, it's easy enough to do a simple search.  If we were to do a search for vSAN disk groups, the address bar looks like this:
```
https://docs.vmware.com/en/search/#/vSAN%20disk%20groups
```

That's cool and gives us results from product documentation, KB articles, technical papers, videos, and even [VMware {code}](https://code.vmware.com/) content.  I could see it making sense to setup a custom search engine to do a search like the one above, but it only saves minimal time.  Like any decent search site, there are additional filters once you see search results.  So I tried clicking one filter and then multiple filters.  But something very odd happened.  The address bar showed an extra parameter called partialfields with no rhyme or reason as to how the value is determined based on the filter you choose.  Here's what the address bar looked like after searching for vSAN disk groups from the Docs home page and then filtering to show only VMware {code} content:
```
https://docs.vmware.com/en/search/#/vSAN%20disk%20groups?partialfields=MYewdgLgppDKIFcBOwoF4BqBZA7gQySgFIAmABiIHYAhUAE2MoBEg
```

![](9_DocswFilters.png)

No matter what filter or set of filers you choose here, there is no way to predict how the address bar will change based on your choice of filter.  That means a custom search engine that will filter the results further is a no go.

Wait a minute!  The Docs site has an Advanced Search area.  You can get to Advanced Search by clicking the icon shown here (will be on the far right side of all search boxes).

![](10_AdvSearch.png)

Unfortunately, the Advanced Search was a bust in terms of custom search engine potential.  Regardless of the parameters selected or words used to search, the address bar gets changed in very odd, unpredictable ways like this:
```
https://docs.vmware.com/advanced-search.html?advanced=N4IghgNiBcIG4GUCCA5ABAEwJYGcDWaA5gE4D2ArgA44gC+QA&lang=en
```

As far as the Docs site is concerned, the only custom search engine potential is for the simple search functionality we showed earlier.  Here are the parameters to use if you're curious (remembering the keyword can be whatever you want) and the accompanying screenshot:

**Search engine:** https://docs.vmware.com/

**Keyword:** docs

**URL with %s in place of query:** 
```
https://docs.vmware.com/en/search/#/%s
```

![](11_SearchEngineDocsSite.png)

 

### Summary

There are many different ways one could choose to setup the custom search engine.  You could choose to setup a few different ones.  But too many will just cause confusion.  That's why I suggest one or two that will be the most helpful.  And these decisions, dear reader, are up to you.  Play around with the All Sources area of the VMware Knowledge Base site a little more if needed.  Technically, a custom search engine could be build to filter search results by product, product version, language, or even date range.  The date range does not seem to use relative references in the browser address bar even though options like today, this week, last week, this month, etc. exist in the filtering area.  I could see a use case for setting a bookmark for a search that shows every blog article with vSAN in the title in the last week, for example, but I think based on what we've seen that is not going to work to filter by date correctly (would need to filter by date once the page loaded).

I pointed out some differences in the Docs site and the Knowledge Base site in previous posts, but when it comes to the way the address bar changes upon conducting a search and filtering the results, they are very different.  In any case, I think there are ways to use what I've written about here to do your job better and faster.

And if you are reading this and are a [vExpert](https://vexpert.vmware.com/), please [sign up](https://goo.gl/forms/2dkj0XUMpD4qyCaT2) to have your blog promoted within [VMware Blog Beat](https://blogs.vmware.com/) so people like me can search your content!

 

### Further Reading

This blog is part 4 of a series on VMware documentation.  Check out the other posts in the series:

- Part 1 - [The Extensive Guide to Using VMware Documentation Like a Pro]({% link _posts/2018-11-30-the-extensive-guide-to-using-vmware-documentation-like-a-pro.md %})
- Part 2 - [Tips and Tricks for Building Customized VMware Docs]({% link _posts/2018-12-07-tips-and-tricks-for-building-customized-vmware-docs.md %})
- Part 3 - [KBs and Beyond – Exploring the VMware Knowledge Base Site]({% link _posts/2019-01-31-kbs-and-beyond-exploring-the-vmware-knowledge-base-site.md %})
- Part 4 - This post
- Part 5 - [Building Custom Search Engines for VMware Content in Mozilla Firefox]({% link _posts/2019-03-29-building-custom-search-engines-for-vmware-content-in-mozilla-firefox.md %})
