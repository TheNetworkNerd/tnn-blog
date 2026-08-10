---
title: "Building Custom Search Engines for VMware Content in Mozilla Firefox"
date: 2019-03-29
media_subpath: /assets/images
permalink: /2019/03/29/building-custom-search-engines-for-vmware-content-in-mozilla-firefox/
categories: 
  - "VMware"
tags: 
  - "Documentation"
  - "Firefox"
  - "Mozilla Firefox"
  - "vExpert"
  - "VMware"
  - "VMware Documentation"
  - "Browser Customization"
image: "featured_Firefox.jpg"
---

In my last post, we explored [Building Custom Search Engines for VMware content in Google Chrome]({% link _posts/2019-02-27-building-custom-search-engines-for-vmware-content-in-google-chrome.md %}).  If we can be VMware documentation search ninjas in Chrome, I thought it would be fun to explore the same idea using Firefox.  Maybe the process will be similar.  We're about to learn the answer.

I'll start by mentioning this post was written while using Firefox Quantum version 66.0.2 (64-bit).  [This article](https://support.mozilla.org/en-US/kb/add-or-remove-search-engine-firefox) details how to add or remove a search engine in Firefox and is pretty simple to follow.  This should be a short post if everything works as expected, right?

### Shortcomings Examined

The article above shows an example of adding YouTube as a search engine from the address bar or from the browser bar.  And it works...for YouTube.  Try something like rakuten.com or etsy.com.  You will quickly find that despite these specific sites allowing search functionalities, they cannot be added as a custom search engine to Firefox using the methods seen in the article.  Try visiting the [VMware Docs Site](https://docs.vmware.com/) or the [VMware Knowledge Base Site](https://kb.vmware.com/s/).  Neither one can be added as a custom search engine using the methods we were shown.

Wait a minute...what if we just go into Firefox options and look at the search engine list?  Shouldn't we be able to add one from there?  I encourage you to try it.  You will quickly find that this is quite different from Google Chrome.  There's not a way to add a custom search engine from here.  We could click on "Find more search engine options" to see if something like what we want exists already.  But, indeed it does not.  With this method we've hit a brick wall.

![](1_Firefox-Options.png)
 

### Searching for a Different Approach

At this point I thought maybe there would be some way to programmatically add the custom search engine(s) or edit a config file to get the functionality I wanted and started looking for answers.  Then I stumbled on [this post](https://superuser.com/questions/1269805/how-to-edit-search-engines-in-firefox-quantum).  It sounded pretty simple, so I wanted to try the steps mentioned to confirm they work.

First, create a bookmark for the [VMware Knowledge Base site](https://kb.vmware.com/s/) in Firefox.  That's no problem.  Notice I can add the site name and any tags I want but nothing else right now.

![](2_MakeBookMark.png)

 
If I open the bookmarks bar, I can easily spot the bookmark that was just saved.

![](3_ConfirmedBookmark.png)

Right-click the bookmark, and go to its properties.  This looks familiar but a little different from what we saw in Google Chrome.  The location is what you would expect, but there's now an option for a keyword.

![](4_BookmarkPropertiesBlank.png)

When we were working in Chrome, we had 3 fields to work with for a custom search engine:

- Search engine
- Keyword
- URL with %s in place of query

To translate that into Firefox's parameters, the keyword has the same meaning.  This will be what we type in the Firefox browser address bar to begin our search.  The tags field won't do much to serve our purpose, so ignore it.  But that location field has to be tweaked.  The location field in Firefox should be the same thing as "URL with %s in place of query" was in Chrome.  Here are the parameters in plain text.  These parameters will allow you to perform a search of the VMware Knowledge Base site and search only Docs and Blogs, sorting the results by relevancy.  Click Save when your changes are complete.

**Location:** 
```
https://kb.vmware.com/s/global-search/%40uri#q=%s&t=MoreContent&sort=relevancy&f:@commonsource=\[Docs,Blogs\]
```

**Keyword:** vmdoc

And if you prefer a screenshot, here it is:

![](5_BookmarkPropertieswithSearch.png)

 
### Checking Our Work

Now, let's see if we were right with our parameters.  If you followed the steps from the blog on [custom search engines in Chrome]({% link _posts/2019-02-27-building-custom-search-engines-for-vmware-content-in-google-chrome.md %}), you know we used these exact parameters as an example.  Call that our control group.  Let's see if we get the same list of results from using our new custom search in Firefox.  Try typing **vmdoc @title=”vSAN disk groups”** and then press enter in Chrome, and then do the same thing in Firefox.  Are the results the same?  They should be if we have configured everything correctly for both browsers.  A similar method can be used in Firefox to build a custom search engine (or bookmark / search shortcut) for the [VMware Docs site](https://docs.vmware.com/).
 

### A Tangent on Config Files

While I was looking for ways to add search engines, I learned the config files for Firefox that control custom search engines are inside a .mozlz4 file named search.json.mozlz4.  When searching for this file on a Windows 10 computer, it was located inside C:\\Users\\%username%\\AppData\\Roaming\\Mozilla\\Firefox\\Profiles\\4xjhkwhr.default.  I'm not sure if the path will be exactly the same on every computer or if the .default directory is made with a random character string.  The path appeared to be the same for all users of a specific Windows 10 computer I used for testing.  The .mozlz4 file mentioned above is not something you can just crack open with a text editor.  This a compressed file that can be decompressed, edited, and recompressed if you want to go there.  [This post](https://support.mozilla.org/en-US/questions/1244862) speaks to that a little if you're interested.

I ended up finding a Firefox Add-on called [mozlz4-edit](https://addons.mozilla.org/en-US/firefox/addon/mozlz4-edit/) that will allow you to add a custom search engine to Firefox.  The icon as shown in a browser looks like this:   
![](6_mozlz4-edit.png)

When you launch the Add-on, you can click Open file, and then navigate to the location of search.json.mozlz4.

![](7_OpenFile.png)

Once you open search.json.mozlz4 using the plugin, you can see the file details in the far right of your browser window.

![](7.5_OpenFileDetails.png)

Below that you see lots of code.  But the very beginning of the code (which looks to be JSON) shows us the same search engine list we saw inside Firefox's options that could not be edited.  Notice the items in this list are **visible** default engines.

![](8_VisibleEngines.png)

That tells me there are more engines baked in but that just don't show up for use.  If you look through this file you will see there are a number of engine definitions.  Only the "\_shortName" parameter of each search engine is listed in the above screenshot.  Here's a search engine for Amazon Search Suggestions that isn't visible but is defined in our search configuration file nonetheless:

![](9_HiddenEngine.png)

 
If you go back to the top set of buttons and click on Configuration, there is more code to analyze when the window shown below opens.  It looks like a master list of search engine URLs, and if you read carefully, this is a copy of a JSON file posted on GitHub!

![](10_Engines.png)

I know this last part may not have been as relevant, but I found it interesting.  Hopefully you did as well.  I'll also add that I found this plugin before I saw the post on how to edit a bookmark and leverage a keyword to create a custom search engine, so I am thankful I ended up doing things the easiest way possible.

 ### Further Reading

This blog is part 5 of a series on VMware documentation.  Check out the previous posts:

- Part 1 - [The Extensive Guide to Using VMware Documentation Like a Pro]({% link _posts/2018-11-30-the-extensive-guide-to-using-vmware-documentation-like-a-pro.md %})
- Part 2 - [Tips and Tricks for Building Customized VMware Docs]({% link _posts/2018-12-07-tips-and-tricks-for-building-customized-vmware-docs.md %})
- Part 3 - [KBs and Beyond – Exploring the VMware Knowledge Base Site]({% link _posts/2019-01-31-kbs-and-beyond-exploring-the-vmware-knowledge-base-site.md %})
- Part 4 - [Building Custom Search Engines for VMware Content in Google Chrome]({% link _posts/2019-02-27-building-custom-search-engines-for-vmware-content-in-google-chrome.md %})
- Part 5 - This post
