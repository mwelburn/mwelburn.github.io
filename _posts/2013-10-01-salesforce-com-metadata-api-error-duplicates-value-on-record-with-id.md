---
id: 476
title: 'Salesforce.com Metadata API Error: Duplicates Value on Record with ID'
date: 2013-10-01T09:00:13+00:00
author: Michael Welburn
guid: http://michaelwelburn.com/?p=476
permalink: /2013/10/01/salesforce-com-metadata-api-error-duplicates-value-on-record-with-id/
categories:
  - API
  - Programming
  - Salesforce
tags:
  - Metadata API
  - Salesforce
---
While using the <a title="Force.com Migration Tool" href="http://wiki.developerforce.com/page/Migration_Tool_Guide" target="_blank">Force.com Migration Tool</a> to migrate an entire organization&#8217;s changes to another, I ran into an issue with the Account object. I seem to have much less success grabbing entire standard objects than adding a few pieces, but it was necessary so I needed to resolve it. The issue was the following:

> Error: objects/Account.object(Account):duplicate value found: <unknown> duplicates value on record with id: <unknown>

<!--more-->

One of the annoying parts of moving a lot of changes in one change set is that if one piece fails that others are dependent on, the subsequent components will also fail to deploy. In my case, I had 25 objects (among other things) ready to import. I was able to remove the Account.object file from my deploy folder structure (and the package.xml listing under <CustomObject>) and then comment out any references to Account relationships in other fields that were erring in order to get the rest of the objects migrated. Then I was ready to tackle the Account, without holding up any of the other people looking to get started in the target organization.

As usually, the first thing I did was paste that into Google. Interestingly, this very cryptic message was actually resolved by someone on <a title="StackExchange" href="http://salesforce.stackexchange.com/questions/6063/object-deploy-fails-with-duplicate-value-found-unknown-duplicates-value-on" target="_blank">this StackExchange post</a>. The answer is incredibly that there is an issue with the History Tracking on the Account object, and you need to turn it off before the import (how anyone derived that is another story). What I ended up doing was opening up the Account.object file, doing a find and replace for all of the following, ensuring that I kept track of the ones I found:

> <trackHistory>true</trackHistory>

Then I flipped each to false and ran my deploy as planned. Once the object was imported to the target organization, I went in and turned on History Tracking on the Account object and manually turned on History Tracking for the individual fields that I had turned off in the Account.object file. While this isn&#8217;t quite ideal, and I didn&#8217;t seem to have an issue with other objects, it beats recreating things manually.
