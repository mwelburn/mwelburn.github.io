---
id: 9
title: Salesforce.com Data Loading options
date: 2013-02-12T00:26:06+00:00
author: Michael Welburn
guid: http://michaelwelburn.com/2013/02/12/salesforce-com-data-loading-options/
permalink: /2013/02/12/salesforce-com-data-loading-options/
categories:
  - Salesforce
tags:
  - dataloader
  - dataloaderio
  - Salesforce
  - Sonoma Partners
redirect_from:
  - /post/42881405128/salesforce-com-data-loading-options/
  - /post/42881405128/
---
I wrote a blog titled <a title="Loading or Manipulating Large Amounts of Data in Salesforce.com? Here's Some Options!" href="http://blog.sonomapartners.com/2013/02/loading-or-manipulating-large-amounts-of-data-in-salesforcecom-heress-your-options.html" target="_blank">Loading or Manipulating Large Amounts of Data in Salesforce.com? Here&#8217;s Some Options!</a> for my company, <a title="Sonoma Partners" href="http://www.sonomapartners.com/" target="_blank">Sonoma Partners</a>, regarding my experience leveraging different tools for loading data into (and exporting data out of) Salesforce.com. There are a number of options out there, and not a lot of resources that give a quick snapshot of what each tool is useful for, so hopefully this is a good reference.

<!--more-->

> When manipulating or loading large sets of data, there comes a point where manual updates are no longer the best option. Salesforce.com provides a couple of different ways to load data automatically based on CSV files, but the most flexible (official) tool they have is <a href="http://wiki.developerforce.com/page/Data_Loader" target="_blank">Data Loader</a>.
>
> Data Loader allows administrators to insert, update, delete, and export records into/from their organization. If similar data loads are done on a recurring basis, administrators have the ability to save the data mappings so they don&#8217;t need to be manually remapped each time. It also has the added ability to run as a scheduled job in Windows via the command line.
>
> For the most part, Data Loader can handle what users need. However, there are some limitations.

It also gave me a chance to highlight <a title="Dataloader.io" href="https://dataloader.io/" target="_blank">Dataloader.io</a>, the tool I use most frequently these days. After using Salesforce.com’s <a title="Data Loader" href="http://wiki.developerforce.com/page/Data_Loader" target="_blank">Data Loader</a> for the majority of my work, I’ve found Dataloader.io to be a great user experience. It is a cloud-based tool that is very easy to use for normal data loading tasks, and a very simple but elegant interface.

For a rundown of the other options, read the rest <a title="Loading or Manipulating Large Amounts of Data in Salesforce.com? Here's Some Options!" href="http://blog.sonomapartners.com/2013/02/loading-or-manipulating-large-amounts-of-data-in-salesforcecom-heress-your-options.html" target="_blank">here</a>.
