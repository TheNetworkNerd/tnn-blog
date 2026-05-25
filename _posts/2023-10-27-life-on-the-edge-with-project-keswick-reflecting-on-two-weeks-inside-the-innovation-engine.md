---
title: "Life on the Edge with Project Keswick: Reflecting on Two Weeks inside the Innovation Engine"
date: 2023-10-27
media_subpath: /assets/images
permalink: /2023/10/27/life-on-the-edge-with-project-keswick-reflecting-on-two-weeks-inside-the-innovation-engine/
categories: 
  - "Career"
  - "VMware"
tags: 
  - "Azure"
  - "Azure Arc"
  - "Building Strategy Prototype"
  - "Containers"
  - "CTO Ambassador"
  - "Desired State"
  - "Development"
  - "Documentation"
  - "Edge"
  - "Edge Computing"
  - "ESXi"
  - "Finding Bugs"
  - "Git"
  - "GitHub"
  - "Innovation"
  - "Intel NUC"
  - "Kubernetes"
  - "Lab"
  - "OCTO"
  - "Product Bugs"
  - "Product Feedback Product Design"
  - "Project Keswick"
  - "Sponsorship"
  - "Strategy"
  - "Systems Administration"
  - "Take2"
  - "Tinkering"
  - "Ubuntu"
  - "VMware"
  - "VMware Ai Labs"
  - "VMware ESX"
  - "Windows"
  - "XLabs"
  - "Yaml"
image: "backpack-1381726_640.jpg"
---

Deep inside VMware's Office of the CTO (or OCTO as we lovingly call it) lies an innovation engine, a secret weapon you might not realize exists.  It's called xLabs, a team of brilliant minds working on new innovations for customers, partners, and employees that are 1-3 years ahead of roadmap (now part of [VMware AI Labs](https://www.vmware.com/artificial-intelligence/ai-labs.html)).  Earlier this year I was blessed to have the chance to work as part of this team for two weeks.  This is the story of how it came to be and what I learned.

### From Interest to Action

Thanks to my participation in the CTO Ambassador program over the last few years, I've been kept up to speed on what is happening across the nooks and crannies of OCTO.  When you learn about some of the innovations and problems we're trying to solve, it keeps you excited about the technology.  It's not that enhancements to existing products aren't interesting, but I like hearing about things that are in the research and ideation stage and those that are in the early prototyping phases.  I had the pleasure of working with the xLabs team in a collaboration with one of my customers for a large portion of 2022 and got a bit more context on what they do.  I've even thought about possibly seeking a full-time role inside OCTO one day.

Based on years of service, one of the benefits VMware offers to its employees is a program called Take2.  It's a 2-week stint where you get to do a completely different job with 100% focus.  It's no different than taking a 2 week course to learn new skills.  Different teams within the company create a requisition with a description of their needs and what the applicant would do during the 2 week period.  And if your application is accepted and your manager approves, you're in.  It's a win for the team who opened the requisition because they get free help, and it's a win for the employee who applies.

I knew a little about the Take2 program but had never applied for anything.  After taking a quick browse of the Take2 application portal I didn't see any openings for my area of interest.  We talk a lot on the [Nerd Journey](http://nerd-journey.com/) podcast about making what you want known, so with that in mind and xLabs being so interesting to me, I reached out to the director of the team to talk about whether she would sponsor a Take2 and if so, what that might entail.  When I sent that e-mail I kind of wondered, "do I actually know enough to add value to a team like this?"  Instead of being held captive by impostor syndrome, I asked the question.  All anyone could say was no, right?

Kudos to Tasha Drew for saying she would support it and for taking that call with me.  She asked me whether I wanted to use the time to learn something new or wanted to apply the skills I have as a solution engineer and former systems administrator to help a specific team.  I told her I wanted both.  I was in it to learn something new and to provide added value to one of the teams in xLabs.  From there, Tasha shared a couple of projects xLabs was working on that could use the help, and she encouraged me to go talk to the product managers for each team.  Once I let her know which team I wanted to work with she would sponsor opening a requisition for a Take2 on that team.

There is one more step here that cannot be overlooked.  You don't do something like this without the support of your boss.  Asking for what you want needs to get to your manager's ears too.  But the process can be made easier if you give them everything needed to make a decision in one shot.  I let the boss know this was what I wanted to do and that I had support from xLabs leadership to do it.  He supported it 100%, and we talked through some dates that made the most sense to ensure team coverage and allow my focus on this special project.

 

### Choosing a Project

Here's where it gets interesting.  I had it narrowed down to 2 projects after speaking with Tasha.  Both were extremely interesting.  Both were chances to work with amazing people.  Both were chances to learn something new.  The product managers / leads of each project shared details about their project and what they thought I could work on within a 2-week period.  The product manager for one of the projects was Alan Renouf, the very same guy who sniff tested an idea I had for the 2017 VMworld hackathon before I submitted the idea and started a team.  I had met Alan in person at Explore 2022 and got to pick his brain a little on his career path, and the chance to work on a project with him was what tipped the scales for me.  Sometimes we take opportunities for the chance to work with and learn from certain people.  This one was no different.  I was on course to do my Take2 on Project Keswick.  If you would like to read up on what Project Keswick is, the problems it solves, and possibly get involved...check out these articles:

- [Edge Management at Scale: Announcing Free Availability of VMware Project Keswick](https://octo.vmware.com/edge-management-at-scale-announcing-free-availability-of-vmware-project-keswick/)
- [First Look at Project Keswick](https://williamlam.com/2023/08/first-look-at-project-keswick.html)
- [Getting Started with Project Keswick](https://docs.vmware.com/en/VMware-Tech-Showcase/services/project-keswick/GUID-3DBACE14-E544-4407-9985-F67D4BE3A6EE.html)

Alan shared a more detailed read out of what he and the team needed help with during the 2-week period and opened the requisition for me with support from Tasha.  I applied, and it was approved and accepted by my boss and Alan and Tasha.  It was game on.  In early February 2023, I was to be plugged into the innovation matrix for 2 weeks.  I knew it would be an exciting experience, but I don't think I knew then just how amazing it would be.  If you don't think you can make an impact in just 2 weeks spent on a project, think again, friend.

 

### A Two Week Adrenaline Rush

When you do something like this, there's a little bit of reading and studying that needs to be done in advance to hit the ground running.  After all, I'm no Kubernetes expert, and I wasn't living day to day in the midst of this project's innards.  That was no problem.  The excitement made up for the extra effort required.  Alan and team made sure I had access to all the right systems to plug right in to help.

We often hear the term strategy and don't always know what that means.  From a product management standpoint a strategy is about how you will accomplish a vision.  One of the things I got to help with was to research and recommend ISV partners in the greater ecosystem that would align with the goals and go to market of Project Keswick.  This involved looking at possible integration points, looking at market share for different tech vendors and learning more about their product capabilities, leaning on knowledge gained from supporting customers, understanding some of the future go to market strategy for the project, and much more.  It was a fun exercise, and though the final decisions on partner strategy for this project were made way above my level, playing a part in it was extremely rewarding.  Several months later I got to see the final partner strategy and was happy to see some of my recommendations made the different focused engagement phases.

There were daily standups with the team working on Keswick, and I was able to attend, ask questions, and give feedback.  These consisted of a product manager (sometimes more than 1), several engineers (i.e. developers), and an engineering lead.  I got to see how the team managed active work in progress, how sprints were organized, how the team would demo their accomplishments at the end of a sprint, how user stories were crafted based on epics (read [this article](https://www.delibr.com/post/epics-vs-user-stories-whats-the-difference) for details on the difference), how bugs were submitted / prioritized / resolved, and some of the challenges the team was running into along the way.  There were sprint post mortems and user experience design reviews too.  The team had to build the customized bits to install on edge devices (a lightweight version of ESXi that will install on edge devices) as well as the back end SaaS platform for managing these edge devices at scale.  I even got to share some of the work I had done in the daily standups.  They treated me just like any other member of the team, even if at times I was intimidated by just how little I knew compared to their depth of knowledge.  It was fascinating, refreshing, and gives you a great appreciation of what product teams do.

When a project is in the incubation stage like Keswick was at the time, they needed people to do test deployments and give feedback.  I spent time installing the Keswick bits on an Intel NUC at the house and made recommendations for ways to improve the documentation.  I had some definite facepalm moments and made simple mistakes, but I learned from them.  I even got to create some bug reports for errors I found, and the team showed me the proper format and details they needed within the bug report so they could work it to resolution.  If you're submitting a bug report (or even just a support request), the more information and context around what the problem was and how you produced it (i.e. screenshots, log files, good descriptions of what worked and what did not) makes it so much easier on personnel tasked with resolving these issues.  Taking more time to provide greater detail up front will improve the quality of experience for you as the submitter and the person on the other side trying to fix the problem.  People often overlook this one when submitting requests for break / fix help.  And sometimes writing all of this down helps you see where you have made a mistake or have a gap in understanding.

Speaking of test deployments, the xLabs team was working with a technology design partner who was planning to use Keswick for one of their customers.  I enjoyed hearing their use case first hand (fire suppression in wind turbines), how they planned to deploy it for their customers in conjunction with other technologies, the challenges they ran into, and the feedback they gave to the Keswick team.  I was able to provide them some guidance on how the greater VMware ecosystem of solutions could integrate with what they already planned to do as well.  I wasn't just a passive listener in this meeting.  I got to be an active participant.  It would have been fun to join additional touchpoints after the Take2 was complete to track progress, but I wasn't able to do that (time constraints with the day job).  If you read the first article linked above, I can tell you it was one of the partners called out by name in the blog.

The Keswick team also needed someone to create a working prototype of Project Keswick integrated with a product from one particular hyperscaler (Azure Arc, which I had never used before).  When someone gives me a task and tells me to go figure something out / make it work and the problem is interesting, it's like drinking a couple of espresso shots.  My energy was high.  It was high enough to tinker throughout the day and into the night, seeking to crush that interesting problem.  It took a little time to get working, but I built a working prototype integrating Azure Arc with a Windows VM, a Linux VM, and the Kubernetes cluster running on my Keswick host at the house.  But it wasn't just me who might need to make the integration work over a longer team.  I wrote a couple of very detailed documents for others who wanted to recreate the integration (with screenshots, of course), detailing where I ran into errors and how I got past them.  But my prototype demo worked when I got to show it to Alan.  I made sure before rolling off the project that I delivered those documents back to the team (to the point of working on finishing touches to them the morning I was about to get on a cruise ship).  Alan probably thought I was crazy (as did my wife), but I wanted to make sure I finished the job.

 

### Reflecting on the Experience

Every person I worked with during this experience was happy to answer any and all questions I had about how Keswick worked, about Kubernetes concepts I didn't quite understand, about internal processes, or about agile processes.  On one of the first days of the Take2, I met with one of the developers who encouraged me to participate in something called an end of day update (or EoD update as people called it).  Each team member would post a summary of their progress that day along with any milestones or blockers along the way in a specific Slack channel.  It was a tool to crowdsource the blockers but also something for transparency and to help me remember what I worked on that day.  It's a way to make the work team members do more visible and a great way to document accomplishments.  Often times people would chime in on my update, ask questions, or make suggestions.  Thinking back, this EoD update might be the product team equivalent of Evan Oldford's daily update from [this episode of Nerd Journey](https://nerd-journey.com/ease-the-mind-through-daily-reporting-with-evan-oldford-1-2/).

Even on a short project like this, you make connections.  In one case, I was able to get one of the developers from the project to do a team training on APIs and Postman, which I thought was excellent.  And I got to meet one of the other product managers in person at Explore this year.  But you also never know where those connections might take you in the future.

I was able to spend some time a couple of months after the Take2 working on Keswick and reporting bugs once they had the SaaS orchestrator up and running.  It was incredible to see how far things had come since my Take2 ended, and it's exciting to see how Keswick is now part of the [VMware Tech Showcase platform](https://www.vmware.com/showcase.html) announced at this year's VMware Explore.  I'm thankful to have played even the smallest part in its development because I think it is a really useful solution for customers and partners alike.  And I learned way more about edge computing than I knew before getting into this project.

Taking 2 weeks to do something totally different is an incredible experience.  You can bring back the perspective gained and the learning to your daily work, and believe me I did.  After working closely with a product team like this, I see so much more clearly the reason we need to consistently get product teams talking with customers and partners.  These teams need to make sure they are building the right product that is scalable, secure, easy to use, and solves the right problem.  If you ever have the chance to give feedback to a product manager / product team (whether positive or an extreme criticism), take the time to do it.  Your feedback has the potential to make the product better for everyone.

Would I do something like this again?  I definitely would...in a heartbeat.  I'm thankful to everyone who supported this experience because it completely re-energized me at a time when I needed some revitalizing.  The Take program exists to give you a taste of what it might be like to do a totally different job.  We can learn about roles in job interviews and discussing with a hiring manager or members of the team, but you don't often get to go try it out to see if it's really something you would like.  I feel like if nothing else it opened my eyes even more to see what people inside our Office of the CTO really do on a daily basis and what it might be like to work in an organization like that someday.  And it's both fascinating and incredible.  If you ever get the chance to work in a different area for a little while, go and do it.  You never know what might happen that will make you better.
