---
title: "Disaster Recovery on the Fly is a Disaster Waiting to Happen"
date: 2012-06-08
media_subpath: /assets/images
permalink: /2012/06/08/disaster-recovery-on-the-fly-is-a-disaster-waiting-to-happen/
categories: 
  - "Disaster Recovery"
tags: 
  - "Disaster Recovery"
  - "Spiceworks"
  - "SQL Server"
  - "SQL Server 2005"
  - "SQL Databases"
  - "SQL Database"
  - "SQL"
  - "Documentation"
---

*Originally posted in the [Spiceworks Community](https://community.spiceworks.com/topic/232181-disaster-recovery-on-the-fly-is-a-disaster-waiting-to-happen) on June 7, 2012*

The story you are about to read is true. There was no need to change the names to protect the innocent. My name is not Friday, and I carry no badge. But, like many others, I am IT.

#### Thursday, April 12 – 11 a.m.
Today is the first day back after a week’s vacation, and the ERP system is acting up all of the sudden. A couple of server reboots later, I close that ticket. Once the database server reboots, I notice two different drives are predicted to fail soon. The HP utility on my server confirms they will fail soon. The databases on this server contain the very lifeblood of the company and cannot afford to be offline if at all avoidable. So, I order a couple of replacement drives (15K SAS, hot swap, just like the ones in the server) to be delivered the next day.

#### Friday, April 13 – 10 a.m.
The drives arrive as planned. The one in my direct attached MSA 70 is a no-brainer. I replace it, and the drive gets rebuilt because it is part of a RAID 1+0 array. It’s a cakewalk.

And then it happens. That sinking feeling overtakes my stomach. I begin to sweat. I want to scream, but it won’t do any good. Maybe if I punch someone, I will feel better. Time slows down. I come to the shocking realization that the second drive that needs to be replaced is one of two drives that were setup in a RAID 0 array. And to make matters worse, this particular logical drive contains the OS partition. Call it oversight. Call it a stupid decision on the part of someone. Call it whatever you want, but it is… reality.

Sometimes in IT we inherit things that were not initially set up the best way. This was one of those configurations I inherited. I tell my boss, and he’s adamant the array can't be RAID 0.

“I assure you it can, and it is, Mr. Boss Man.”

He thinks it over, lets the truth and severity of the situation sink in, and says, “You got this?”

I give him the affirmative nod. Sure I do. I’m not a SQL DBA. I’ve never rebuilt a SQL Server, but I’d figure it out — somehow.

Next, I do what any good IT pro would do when faced with a task like this: I look at my DR plan for this server. But, since our department has no DR plan for this server (oops), I do the only logical thing left to do at this point: I tap the Spiceworks Community for advice.

#### 2 p.m.
I have the drives I need ordered and set to arrive on Saturday and keep the one ordered the previous day as a hot spare. At this point I’ve coordinated with production and found myself a very large window of server maintenance time for Sunday afternoon. The plan is to image the problem partition and try to restore it on a shiny new RAID 1 array.

#### Saturday, April 14 – 11 p.m.
I’m thinking about what I might need for the next day’s surgery. As I inspect my SQL Server, I realize there are several DBs with MDFs and LDFs stored on the OS partition (more good news). I back them up one by one. I think about it, and I go ahead and start the Server 2008 R2 install media download from Microsoft’s Licensing Portal — just in case. As always, I hope for the best and prepare for the worst.

#### Sunday, April 15 – 12:30 p.m. (8.5 hours until production starts)
Sunday afternoon arrives. I back up a few other files on the C-drive, and it’s go time. I start to image with Symantec Ghost to an external drive. The RAID 0 partition is only 146 GB in size. After 30 minutes, I see the write speed get slower and slower because I may quite possibly be using THE slowest external hard drive ever made by Western Digital. At this point, the end time is predicted to be about nine hours later. I have other external drives I could try, but they’re all the same speed. It’s gut-check time. I could let the imaging process run for the nine-and-a-half hours, but what guarantee do I have it would restore properly after taking the image? Could some free imaging program like Clonezilla have run faster? I’m not sure but don’t have time to play around.

#### 2 p.m. (7 hours until production)
At this point I make the decision to attempt to beat the clock. The server was built nearly four years ago, so why not start fresh? I decide I can put in the new drives, create a RAID 1 partition, re-install the OS and SQL Server, and have the server back to the way it was in about seven hours. Production won’t miss a beat. Even if I don’t finish, I’ll be better off than waiting on the image to finish and essentially accomplishing nothing.

I power down the server, remove the two drives, put the new ones in place, and then I’m set to configure RAID. It turns out my only configuration options are RAID 0 (not an option), RAID 5 (only have two drives that are the same size to use and don’t want to kill a puppy), and RAID 1+0. RAID 1 isn’t an option. How can you have RAID 1+0 with two drives and not at least four? It turns out the RAID 1+0 option is just RAID 1 when you have two drives. (It may be common sense to some, but thank you SAM for answering that question quickly.)

#### 3 p.m. (6 hours until production)
After RAID is configured, I install Server 2008 on my new RAID 1 partition with no issues (after hunting for and finding the correct license key). Then it’s time to install SQL Server 2005, and I can’t find the media. I wish I was joking. I text my boss after tearing the server room apart, and he’s sure we have it… somewhere. Maybe we do, but it’s not anywhere we usually store software media. It isn’t stored on our file server.

#### 4 p.m. (5 hours until production)
I login to the MS licensing portal and look for SQL 2005 so I can download it, but they only have the 32-bit version in Japanese and not English. I’m feeling like it’s just not my day. Why did I come back from vacation again? Call me crazy, but I double and triple checked the SQL version. I decide to go ahead and download it. The download files are huge because I need SP4 since that is what we have installed. Now I’m faced with sitting to watch the download on a 4.5Mb bonded T1. I take a deep breath and think of one more place the media might be.

Finally, I find everything I need in the Downloads folder of my work laptop. Turns out, I had downloaded it at least a year ago to create a test environment for my boss to do some ERP system testing. I feel a rush of relief. Maybe I’ll get it all fixed before production runs tonight.

#### 5:30 p.m. (3.5 hours until production)
I install SQL 2005 and upgrade to SP4. I restore all my user/system databases using Backup Exec, but I can’t restore the master database with Backup Exec. That has to be done on the server itself with a SQL backup because you have to kill all but one administrative connection to the master database. At this point I realize I didn’t back up the master database with SQL — just Backup Exec. I have to shut down the server and put my old drives back in again to get SQL backups of all my system databases. Then I put the new drives back in and keep going. I still can’t restore the master database. I’ve missed something.

#### 7 p.m. (2 hours until production)
Turns out I’ve named the instance of SQL on my newly rebuilt server something different than it should be for all remote connections to my databases to work. I have to detach all databases and re-install SQL using the default instance name. At this point, I re-attach the user databases, and I can connect to them all remotely. My ERP system still won’t connect to its database, and I still can’t restore my backup of the master database (despite restoring all other system databases successfully).

#### 8:30 p.m. (Production starts in 30 minutes)
I’m clueless, even with Google’s help. I’m frustrated. I decide it’s best to turn back the clock to the old RAID 0 configuration so production can run. Even if that dying drive croaks before the next weekend, I’ve built 90 percent of a new server.

The boss is happy we at least made progress. I document _**everything**_ I’ve done in the help desk ticket I had open for this issue, including a list of what still needs to be done. I go home late in the evening to see my family.

#### The next Week
The dying drive survives all week (thanks to the good Lord), but I can’t sleep well at nights thinking about what might happen if she goes down. I keep thinking about what work is left to do. I decide to finish the job Saturday night after production finishes. My window is nearly 24 hours, and I plan to conquer this time.

#### Saturday, April 21 – 11:15 p.m.
I get the call that the last production guy has left for the night. I back up all the databases with SQL and Backup Exec. I start back with my new configuration and a restore of the system databases. Thanks to Google, I got them all restored and even moved their MDF and LDF files off the C-drive (now stored on my MSA 70 with RAID 10) as a best practice. I restore the user databases and make sure all MDFs and LDFs for those databases are stored on the MSA 70 as well. I can connect to the databases remotely. My ERP system works. Our intranet site that pulls from user databases works. The SQL Maintenance Plan we had in place to back up the ERP system database works. After teaming the NICs on the server and testing again, I back up my databases again with Backup Exec and the system databases with SQL backups.

#### Sunday, April 22 – 3:56 a.m.
I send out a company blast that everything is up and running again. I take the time to document all the remaining steps needed to rebuild the server successfully, and I go home to sleep… finally.

#### In Summary
I learned so many things by completing this project. This is an example of an experience I hope you never have, but I know it happens. Even though I didn’t set up this server initially, I had to fix it, and the fact that I had almost no experience doing something like that for a SQL box and no DR plan to guide me translates to about 400–500 percent of the time that should have been spent on such an exercise. On the bright side, I now have a disaster recovery plan for this server. It’s all in that help desk ticket because I wrote it on the fly, and it didn’t take very much effort.

Sometimes disaster recovery planning gets marked as the lowest priority — until disaster strikes. Hopefully you won’t find yourself in the same boat. If you build it, build it with best practices in mind the first time, or you or someone else will get to build it again.

I want to thank everyone who replied to the following threads. You did indeed save my butt.

[http://community.spiceworks.com/topic/216055-hot-swap-drive-in-raid-0-sas-array](http://community.spiceworks.com/topic/216055-hot-swap-drive-in-raid-0-sas-array) [http://community.spiceworks.com/topic/218166-rebuilding-a-sql-2005-server-best-practices](http://community.spiceworks.com/topic/218166-rebuilding-a-sql-2005-server-best-practices)
