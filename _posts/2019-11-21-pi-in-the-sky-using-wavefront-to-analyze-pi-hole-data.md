---
title: "Pi in the Sky: Using Wavefront to Analyze Pi-Hole Data"
date: 2019-11-21
media_subpath: /assets/images
permalink: /2019/11/21/pi-in-the-sky-using-wavefront-to-analyze-pi-hole-data/
categories: 
  - "Conferences"
  - "VMware"
tags: 
  - "Grok"
  - "Grok Patterns"
  - "Logging"
  - "Logstash"
  - "Metrics"
  - "Pi-hole"
  - "Proxy"
  - "vBrownBag"
  - "VMware Wavefront"
  - "Wavefront"
  - "Wavefront Proxy"
  - "Blog Series"
  - "Public Speaking"
  - "Presentation"
  - "Conference"
  - "Tech Conference"
  - "Conference Presentation"
image: "pi-3414765_640.jpg"
---

What happens when you have an idea for a conference talk but don't have the technical expertise to back it up?  And what if you think the things you plan to do with certain technologies can be done but aren't 100% certain?  Do you submit anyway and risk failure or just save the idea for another time?  That was me when the submissions for [vBrownBag TechTalks](https://vbrownbag.com/2019/05/vbrownbag-techtalks-at-vmworld-submissions-open/) opened for VMworld 2019 US this past May.  The talks could be 12 minutes or 27 minutes but were meant to be short.  Could I get 12 minutes of content out of something I had never actually done?  Maybe...?

I started by creating a formal summary for the talk based on the idea.  Here's what I came up with:

> Many people have adopted Pi-hole as an ad-blocking solution for a home network.  While it did make browsing the web faster at my house, I found the web interface lacking truly useful information.  Rather than dig through logs on my own, I decided to push the logs to Wavefront and do some metrics analysis.  We'll talk about the steps needed to get Pi-hole data to Wavefront and how to use Wavefront for more insight into what's really been happening on your home network.

It sounded like something no one else would be submitting.  I knew from previous years that early submission is best (if I intended to do it).  Submitting in May would give me until late August to learn whatever was needed to give this talk, right?  It would force me to learn something new and give me a deadline.  I thought Wavefront and Pi-Hole were interesting enough to go for it, so I submitted the talk.

At this point I did not know for sure if the submission would be accepted, but that didn't keep me from starting.  Even if the talk did not get accepted, I could still pursue the idea as time allowed.  After some initial research, I reached out to Bill Roth for direction and to get started down the right path on this project.  Kudos to him for letting me pick his brain a little.  I figured the best way to learn was to go step by step and blog about my progress.  That launched a blog series that you can follow starting with [this post]({% link _posts/2019-05-26-creating-a-wavefront-proxy-in-aws.md %}).

### The Biggest Hurdles

As I worked through technical challenges along the way, the two largest were verifying the logs made it to the Wavefront proxy and learning how to use and debug GROK patterns (completely new to me).  I had a lot of help from Vasily Vorontsov at this stage, and I am thankful he was willing to help.

As we all know, running into technical hurdles can cause unexpected delays.  I worked on the screenshots and graphs in Wavefront for this presentation all the way up to the night before I gave it.  In fact, my wife got extremely ill right before I was supposed to leave for San Francisco, but she forced me to leave so I would not miss the chance to give this talk after all the preparation.

I ran through the presentation a few times to ensure I could get it under the 12 minute limit and made a few Powerpoint notes to help me remember certain things.  When they started the timer and told me to go for it (i.e. they started recording and the 12 minute countdown), I quickly realized I could not see the speaker notes in Powerpoint.  But there was no time to change it.  I figured at that point I would either sink or swim based on what I knew.

So here it is...a 12-minute presentation that took many hours of work to create.  I hope it will be helpful to others in the community.  Maybe one day there will be an extended version after I can make more progress.

 {% include embed/youtube.html id='Zo1lNVHLTwg' compact=1 %}

<!-- original link is https://www.youtube.com/watch?v=Zo1lNVHLTwg -->

If you would like a copy of the slides from the presentation above, you can find them on [my GitHub page](https://github.com/TheNetworkNerd/Presentations/tree/master/Pi%20in%20the%20Sky).  I would like to say a special thanks to Aaron Bolthouse, Bill Roth, and Vasily Varontsov for their help as I was pursuing the technical work needed to prepare for this presentation.  Without their help, it never would have happened.

### Further Reading

This blog is part 5 in a series on analyzing Pi-hole log data with Wavefront.  Check out the other posts in the series:

- Part 1 - [Creating a Wavefront Proxy in AWS]({% link _posts/2019-05-26-creating-a-wavefront-proxy-in-aws.md %})
- Part 2 - [Shipping Pi-hole Logs to a Wavefront Proxy]({% link _posts/2019-07-28-shipping-pi-hole-logs-to-a-wavefront-proxy.md %})
- Part 3 - [Verifying Wavefront Proxy Log Ingestion]({% link _posts/2019-08-12-verifying-wavefront-proxy-log-ingestion.md %})
- Part 4 - [Fishing for Wavefront Metrics with Grok Patterns]({% link _posts/2019-08-16-fishing-for-wavefront-metrics-with-grok-patterns.md %})
- Part 5 - This post
- Part 6 - Coming soon!
