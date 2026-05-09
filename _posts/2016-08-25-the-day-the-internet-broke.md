---
title: "The Day The Internet Broke"
date: 2016-08-25
permalink: /2016/08/25/the-day-the-internet-broke/
categories: 
  - "internet-service-provider"
tags: 
  - "finance"
  - "internet-service-provider"
  - "isp"
  - "process-improvement"
  - "time-warner"
---

Originally posted on [MangoLassi](https://mangolassi.it/topic/10450/the-day-the-internet-broke) August 8, 2016

It was mid-day on a normal Wednesday when I started getting alerts that devices have fallen offline at one of our locations. Seconds later the alerts about the firewall at this location falling offline were hitting my Inbox. Something was awry, and I needed to determine exactly what happened. My team and I are physically stationed at the company headquarters but support several remote sites in the area (and some out of state).

I called a user at the location in question (which is just over 30 miles from my office) to see if the facility still had power. He said there were no power issues and even went into our telco room at this facility to check equipment lights. Everything seemed normal on our UPS (not running on battery), firewall, switches, and the ISP's modem (all lights for US/DS and online solid blue).

This location has about 20 users and uses Time Warner coax as their internet connection. For the most part the connection has been reliable, but it is still coax and not as reliable as fiber. Since we couldn't ping the ISP's device at that location, I had the user power cycle the modem to see if that would fix the problem. Once the modem was online again I could ping it for about 15 seconds and then never again. Our firewall never came online, and folks at the location could not connect to the internet.

My mind immediately jumped to possible ISP issues, and I called Time Warner support to see if they might have an outage in the area. After giving the technician all the account information and describing the problem, I hear him pause and then say, "oh, there's the problem. Your account has been issued a soft disconnect because your account is overdue." Honestly that's the last thing I thought could have been the problem in this situation.

The technician transferred me to someone in billing who told me the account was 3 months overdue, and we owed over $1000 to them. I check with Finance, look through all of the billing statements, and find we issued a check on 8/18 for over $1000, which Time Warner had not received and had not yet processed. Even if they had received it, the processing of the check and applying it to the account takes time. Waiting on the check to process was not an option.

Luckily, I was able to pay the minimum amount to get us back online again (a little shy of $700). After the person in billing processed the payment, they had our internet circuit turned on again within 5-10 minutes. Once they receive our check and process it, we'll have some credit on our account for a couple of months.

After all of this I went and spoke with Finance about the situation one more time. It looked like we had received the invoices but that they had been bouncing them around to different people to approve before issuing the check, which caused a bit of a delay. In any case, they are going to look to pay this specific bill using an auto-draft so we won't run into this problem again.

At your company, have you ever had this happen? Do you have a role in approving the bills for recurring charges like internet and phone? What's your process?
