---
title: "Catch the Wave with GCP - A Wavefront Integration Exploration"
date: 2018-04-15
media_subpath: /assets/images
permalink: /2018/04/15/catch-the-wave-with-gcp-a-wavefront-integration-exploration/
categories: 
  - "Cloud"
  - "VMware"
tags: 
  - "Analytics"
  - "Google"
  - "Google Cloud Platform"
  - "VMware"
  - "VMware Wavefront"
  - "Wavefront"
  - "Product Integration"
  - "GCP"
  - "Metrics"
  - "Observability"
  - "Metrics Analysis"
image: "23_Wavefront_CreateEvent.png"
---

Perhaps you've heard some buzz about [Wavefront](https://docs.wavefront.com/wavefront_introduction.html) but don't quite know what it is or how it can be used.  I was in the same boat for a while, but then I saw a demo that blew my mind.  Imagine a [platform](https://www.wavefront.com/analytics/) that could ingest time series data (or metrics) from various sources, retain all data points with no roll up, and allowed IT operations and developers to quickly analyze the data for spotting and visualizing trends affecting application performance.  How could we use that data to make the business better, and what kind of data actually gets collected?  Moreover, how and why are people using this tool?  I hope this post will provide some answers.

### Wavefront Integration with Google Cloud Platform

The Wavefront website shows [a number of integrations](https://www.wavefront.com/integrations/), but what does that really mean?  Wavefront has pre-built methods for metric collection from certain applications and will guide you through the setup process.  There are other ways to [get data into Wavefront](https://docs.wavefront.com/wavefront_data_ingestion.html), but integrations make the process easier.

I happen to have a [Google Compute Engine](https://cloud.google.com/compute/) instance running a single Linux virtual machine, so it made sense to play with the [Google Cloud Platform](https://cloud.google.com/) integration.  [This blog post](https://www.wavefront.com/gcp-metrics/) gives some detail on the GCP integration, but once I read it, I wanted to see it for myself.   Sign up for a [free 30-day trial](https://www.wavefront.com/sign-up/?utm_source=Website&utm_medium=referral&utm_campaign=website-top-ribbon) of Wavefront and a [free year of GCP](https://cloud.google.com/free/) to follow along.

We'll start by logging into a Wavefront with the proper e-mail address and password.  
![](1_WavefrontLogin.png)

After logging into Wavefront, there are a number of tutorials available.  To get started on our mission, click the Integrations option on the top bar as shown here.  
![](2_Integrations.png)

There are numerous [Wavefront integrations](https://www.wavefront.com/integrations/) available, but we're only concerned with Google Cloud Platform in this post.  
![](3_FeaturedIntegrations_Google.png)

After clicking on Google Cloud Platform, you will see something similar to the screenshot shown here.  Since Wavefront is not yet connected to GCP, the METRICS and CONTENT labels below show gray information symbols (no metrics are reporting, no content is installed).  Keep in mind the integration with GCP will bring in metrics for all of the following (if applicable): Google Computing Engine, Google Container Engine, Google App Engine, and Google Cloud Datastore.  Read about the different metrics collected if you like, but click Setup to continue.  
![](4_GCP_ClickSetup.png)

Since there are no integrations with GCP yet, click SET UP INTEGRATION as shown here.  Clicking ADVANCED takes you to a list of all cloud integrations setup with your Wavefront instance.  
![](5_SetupIntegrationButton.png)

I've given the integration a name at this point (Nick's GCP Metric Playground) but need to get some information from my GCP instance before I register the integration.  Let's see if the directions work as expected.  
![](6_RegisterGCP.png)

 
### Dive into GCP

After logging into my GCP account, I followed step 2 shown above from the GCP dashboard.  
![](7_GCP_IAM_Admin.png)

When following step 3, make sure a project is selected.  In the screenshot shown here, the project I selected was called My First Project.  Then choose CREATE SERVICE ACCOUNT.  
![](8_CreateServiceAccount.png)

The service account name from step 4 above can be anything, but I chose to stick with wavefront-integration.  Notice the service account ID is in the form of an e-mail address.  It will be an address unique to your GCP instance.  The Role field below shows as Viewer and not Project Viewer even after following the instructions.  I've included an additional screenshot below showing the dropdown selection process to verify we have completed all steps correctly.  
![](9_ServiceAccountCreation.png)

Steps for selecting the Project Viewer Role are shown here.  
![](10_ProjectViewer.png)

After clicking CREATE on the service account menu above, you will see a message stating the service account and key were created, and the proper JSON file gets downloaded to your computer.  
![](10.5_ServiceAccountKeyCreated.png)

### And Now Back to Wavefront

Now we're ready to go back to the GCP integration page in Wavefront.  Open the JSON file in an editor like Notepad, and paste all of its text in the JSON key field shown here.  
![](11_JSON_Paste.png)

![](19_GCEIntegrationSetup.png)

After pasting the JSON key, you have a choice to fetch all metric categories or narrow the scope (i.e. just pull metrics for App Engine and Compute, etc.).  I suggest leaving all categories selected.  The service refresh rate is how often Wavefront will reach back to the GCP instance to pull in new data.

At the bottom of this page, click REGISTER to save the integration, and hopefully you see a success message similar to the one shown here.  Wavefront is now connected to my GCP instance and should be pulling in data.  
![](12_IntegrationSuccess.png)

At this point, we are directed back to the Google Cloud Platform Integration Page in Wavefront.  
![](13_NameandProjectID.png)

Our integration can be edited at any time (i.e. to collect data more frequently or collect a smaller subset of metrics) by clicking its description in the Name column.  The Project ID is something that comes from the GCP project for which you created a service account.  If you had multiple GCP projects, you could follow the same process to connect them all.


### How Can I See My Metrics?

The integration is complete, and you're chomping at the bit to see metrics.  Go back to the Integrations menu, and you'll see the GCP integration now has a green check mark on it.  This means the integration is actively in use.  Click on it to continue.  
![](14_GCPIcon.png)

This looks different.  We have metrics!  The metrics Wavefront is collecting are being ingested at 1.433 points per second.  The metrics collected are listed by name in the left-hand tree.  
![](15_GCPMetrics.png)

But that's not all.  What if we click on Dashboards?  There are some canned dashboards for GCE, so let's see what we have.  This is only one of several ways to get to these dashboards (search for GCP in Dashboards -> All Dashboards for another option).  
![](16_GCE_Dashboards.png)

In the few minutes the integration has been setup, look at the data I have.  I'm showing you 4 out of 36 different charts in the dashboard giving overview stats (8), network stats (12), and storage stats (16) for my GCE workloads (just a single VM at the moment).  And in a day, a week, a month, etc. all of this data will be retained and searchable should I want to do some comparative analysis.  
![](17_GCE_Dashboards_1.png)

![](18_GCE_Dashboards_2.png)

I can click on the name of any chart to show a single metric and zoom in to any time period for which we have metric data.  For example, clicking on the STORAGE IOPS chart above, I see a default 2 hour chart of the average storage IOPs across my GCE instance that can be changed to my liking.  
![](20_GCEIntegration.png)

### How Does This Integration Help Me?

All of the metrics seen in Wavefront are just those exposed via API from Google.  If we look at the native monitoring tools inside Google Compute Engine for CPU utilization of my Linux VM, I have a few options for time windows, but that's it.  Is that going to help me if performance isn't so great?  Maybe.  
![](21_GCE_CPUChart.png)

What if I wanted to look at CPU utilization for a specific date range or zoom into a window of say 15 minutes when I knew there was a performance spike?  That's where Wavefront is far superior.  The range below shows a 2 hour period, but I can switch to any time period with ease.  
![](22_Wavefront_CPUChart.png)

What if I wanted to...

- Create my own chart for a specific metric?
- Mark events like code pushes on a chart to locate and visualize a performance bottleneck?
- Create a new dashboard?
- Setup an [intelligent alert](https://www.wavefront.com/alerting/)?
- Do analysis to correlate 2 metrics?
- Use query language to create a chart?

My cloud provider does not have the tools for these tasks, but with Wavefront, I can use the metrics collected to make informed decisions based on the results of my detailed analysis.

This is one of many integrations to explore.  Will you catch the wave?

### Helpful Links

- [GCP Free Tier / Free 12 Months](https://cloud.google.com/free/)
- [Google Metrics List](https://cloud.google.com/monitoring/api/metrics#gcp-compute)
- [Wavefront 30-day Trial](https://www.wavefront.com/sign-up/?utm_source=Website&utm_medium=referral&utm_campaign=website-top-ribbon)
- [Wavefront Monthly Webinar](https://vmware.zoom.us/webinar/register/8015231278909/WN_Fuw7mZpdQneJ34MrsDwlPg)
- [Wavefront Public Slack Channel](http://www.wavefront.com/join-public-slack)
- [Wavefront Getting Started Tutorial](https://docs.wavefront.com/tutorial_getting_started.html)
- [Wavefront Product Demo Videos](https://www.wavefront.com/resources/)
- [Wavefront Pricing FAQ](https://www.wavefront.com/pricing/)
