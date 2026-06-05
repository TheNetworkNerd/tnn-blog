---
title: "Spiceworld 2017 Session -The IT Manager's Guide to Shadow IT Prevention"
date: 2017-10-21
media_subpath: /assets/images
permalink: /2017/10/21/spiceworld-2017-session-the-it-managers-guide-to-shadow-it-prevention/
categories: 
  - "Conferences"
  - "Spiceworks"
tags: 
  - "Shadow IT"
  - "Spiceworld"
  - "Spiceworld Conference"
  - "Spiceworld 2017"
  - "Technology Conference"
  - "Technology Conferences"
  - "Tech Conference"
  - "Tech Conferences"
  - "Presentation"
image: "YouTubeThumbnail-SW2017.jpg"
---

This year I was blessed to present a session on Shadow IT at Spiceworld 2017, and I have to say it was an amazing experience.  The best part about it was getting the attendees involved.  I expected questions during the presentation, but I was very grateful for all the audience members who were willing to give some helpful tips from their own experience.  If you watch the video at the end of this post, you will see exactly what I mean.

### What Do You Mean Breakout Sessions Were not Recorded This Year?

When I found out the breakout sessions were not going to be recorded this year, I posted this [call to action thread](https://community.spiceworks.com/topic/2044956-spiceworld-2017-presenters-a-call-to-action) for all Spiceworld presenters in hopes that as many of us who could would video the presentation and post in the community and on YouTube.  Many presenters did just that.  This post is my bid to give you the video, a transcription of some of the best questions and answers, and the questions people sent in via Twitter in one spot.

For those who want more, all you have to do is keep reading.

### Dialog from the Presentation

There are comments from the attendees that you may not be able to make out from watching the video, so I'll transcribe some of them here in the order they happen in the video.

- Audience Definitions of Shadow IT (around 10:00 in video)
    - IT functions that happen outside of the IT infrastructure done by someone other than IT
    - Undocumented and unapproved changes
    - Uncontrolled and undocumented sprawl of systems
    - Forced implementation of software that didn't involve IT in the decision making
    - Users trying to skirt the process to make things easier
    - What happens when the IT department is not the least painful option
    - Implementing IT solutions without including the IT department

- Taking User Suggestions to Heart (17:28 in video)
    - They know the job.  You don't.

- How Do You as an Admin Prevent Yourself from Shadow IT Actions? (29:05 in video)
    - I talked about not having full access to the SQL database where corporate payroll lives.
    - Justin Davison suggests rotating department members who audit specific systems on a particular cadence (i.e. firewall rule audit every 6 months) to catch bad actors.
    - Another audience member mentioned using [Netwrix](https://www.netwrix.com/) to audit AD, SQL, and several other systems to highlight changes.
    - **Not in the video:** Something else here would be to not exempt your own computer from software inventory and software acceptable use policies.  Work with your team on a standard list of programs the IT team will use in addition to the average corporate end user.  If you're using something that is not authorized, that's effectively Shadow IT.

- Scenario - People Bypassing Change Management System in Small Department (37:28 in video)
    - In this scenario, the change management system is for all departments to track changes to a process, a system, or anything else.
    - Since you have a baseline of how things should be, any change not documented in the change management system gets reverted.  When someone asks why the change was reverted, direct them to enter the change into the change management system and provide excellent service.
    - Use the stick - terminate people that continually go around the system (manager or subordinate).  Quantify the cost of fixing disasters created from undocumented changes.  One audience member describes the power of firing someone publicly and communicating the reason for doing it.
    - Use the string - if you get too harsh with people, they will be afraid of you (which can promote Shadow IT).  Go and talk to the person to find out why they are not using the system (i.e. do they find it cumbersome, is it a lack of training, etc.).
    - Learn to talk to the users.  That side is just as important, and maybe there is something being missed that can help.
    - Every time you do something, there are features and benefits.  Benefits are more important to users and C-levels.
    - The change management system may be too difficult for folks to use and might not be the right product.
    - In order to make a change, someone needs the proper permissions.  Maybe the permissions should be removed for folks going around the system until they can understand why the system is in place, why changes need to be documented, and follow proper procedure.
    - **Not in the video:** Make sure training for using the change management system becomes part of the user onboarding process so people can hit the ground running when they begin employment.
    - Access for vendors and even for people in other compartments can be on-demand.  You can turn it on when the change request is submitted and turn off once everything is completed.

- Scenario - One Man Department Getting Bypassed by Partners for Technology Implementations  (47:12 in video)
    - One audience member says the partners / owners don't have the same rules as normal end users when it comes to getting IT support.  Another audience member encourages having a discussion if there is a question and using tickets if there is an action required on the part of the IT department.
    - You must talk to the partners at their level.  Encourage them to run decisions by you to ensure they get the best solution for the company.  Concentrate on learning how they talk and how to talk to them.  It may take time, but the decision makers will come around.
    - It's ok to say things like "we can do this, but..." / "we can do this, or..." to help steer the project in a different direction without telling them no.
    - What if you have done all this but the decision makers still think they know better?  Continue to work with them, and be patient.  Use their pain and problems to your advantage.
    - Don't be reactive, but seek to be proactive.  Develop a relationship with end users, C-levels included.  Get out there and talk to them.  Make it personal.
    - It's important to approach all situations with humility.  We aren't going to be able to solve every problem, but we need to see relationships with other departments as a partnership.  Take the feedback that your department may not be meeting the needs of other departments, and change things to work to meet those needs.  As you do this, other departments will be more patient and understanding with you.
    - **Not in the video:** If nothing changes over time for the better (i.e. decision makers still think they know better than you and never take your suggestions), start looking for another job.  Hopefully you did your best to value sell having an internal IT resource, but if they continually do not see the value, go somewhere that will.

 

### Questions and Comments from Twitter (#SW2017ShadowIT)

- An attendee recommended looking into Splunk for centralized logging and possibly even SIEM.

- What's the best way to see if someone is using Dropbox, Drive, etc. with unauthorized company data?
    - Monitor to see if these applications are installed on workstations (i.e. software inventory) or if users are visiting the sites for these services on company computers.  If you have the right filtering technologies in place for company computers, it is possible to block all traffic to these services regardless of where someone takes their laptop.  But I will also say companies like Dropbox might tell you which users within the company have registered with the service using a company e-mail address.  If you can prove the software is installed and that employees are using Dropbox personal plans registered to company e-mail addresses, this could be great leverage for getting budget to purchase a business plan with administrative controls and auditing.

- What is an appropriate balance between enforcement via HR policy and enforcement via tools (i.e. web filters, USB blocking, SRP)?
    - It all comes down to the acceptable level of risk to the company.  If you can get a handle on how much Shadow IT is happening (through software inventory, looking at browsing habits of users, using a SIEM, using syslogs, listening to user pain points, etc.) and how it is affecting the business (i.e. out of regulatory compliance, out of licensing compliance, risk of malware, etc.), you can start to do something about it.  Present the current situation to management, HR, QA, and your operations team.  Propose a policy and help write it, and then use appropriate tools to enforce the policy.  If management doesn't feel like blocking USB is needed, and it doesn't put you out of compliance, you don't do it.  If it would put you out of compliance, go back and talk with management about why you truly need to do it.  If they don't listen, get QA and HR involved to help.

 

### And Now for the Video...

![video link)(https://youtu.be/wpiAKmmP15U)

{% include embed/youtube.html id='wpiAKmmP15U' %}
