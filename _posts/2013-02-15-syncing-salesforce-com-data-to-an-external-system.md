---
id: 8
title: Syncing Salesforce.com Data to an External System
date: 2013-02-15T14:00:38+00:00
author: Michael Welburn
guid: http://michaelwelburn.com/2013/02/15/syncing-salesforce-com-data-to-an-external-system/
permalink: /2013/02/15/syncing-salesforce-com-data-to-an-external-system/
categories:
  - Programming
  - Salesforce
tags:
  - Apex
  - Asynchronous
  - callout
  - Dynamics
  - JSON
  - MSCRM
  - Salesforce
  - Sonoma Partners
  - sync
  - trigger
redirect_from:
  - /post/43146905925/syncing-salesforce-com-data-to-an-external-system/
  - /post/43146905925/
---
I recently wrote a blog titled <a title="Our Story: Synchronizing the Sonoma Salesforce.com and MSCRM Dynamics Environments" href="http://blog.sonomapartners.com/2013/02/our-story-synchronizing-the-sonoma-salesforcecom-and-mscrm-dynamics-environments.html" target="_blank">Our Story: Synchronizing the Sonoma Salesforce.com and MSCRM Dynamics Environments</a> about an internal project that I did at <a title="Sonoma Partners" href="http://www.sonomapartners.com/" target="_blank">work</a> regarding syncing data from Salesforce.com into a Microsoft Dynamics system to create a single master system.

<!--more-->

> As a partner of both salesforce.com and Microsoft, we have separate sales teams handling each product using their respective CRM tools, salesforce.com and Microsoft Dynamics CRM. Unfortunately, this leads to having two &#8220;systems of record&#8221; in terms of customer base, sales pipeline/history, and contact management.
>
> In the past, our salesforce.com teams have had to manually re-create records in Dynamics CRM, which robbed them of valuable time. To remedy this, we built an in-house solution to synchronize our CRM data into a single system of record, which we chose to be Dynamics CRM for a couple reasons. We have done some robust customizations to our internal system over the last few years to really make our process flow fit our business, and we also use it as the focal point of our internal project management and time tracking. Instead of rebuilding any of these pieces out again in salesforce.com, it made the most sense to leverage our existing work and push our salesforce.com projects into Dynamics CRM when we needed to start taking advantage of our other solutions.

While this implementation was nowhere near as complex of an integration as it could have been (no security precautions, one-way sync, etc.), it was still an interesting example of how to architect data dumping out of Salesforce.com automatically using triggers, <a title="Future Methods" href="http://www.salesforce.com/us/developer/docs/apexcode/Content/apex_classes_annotation_future.htm" target="_blank">future</a> methods, JSON serialization, and HTTP callouts.

Read more <a title="Our Story: Synchronizing the Sonoma Salesforce.com and MSCRM Dynamics Environments" href="http://blog.sonomapartners.com/2013/02/our-story-synchronizing-the-sonoma-salesforcecom-and-mscrm-dynamics-environments.html" target="_blank">here</a> if you are interested.
