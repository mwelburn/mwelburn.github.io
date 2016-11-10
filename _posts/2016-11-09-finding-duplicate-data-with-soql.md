---
title: Finding duplicate data with SOQL
date: 2016-11-09T23:11:06+00:00
author: Michael Welburn
permalink: /2016/11/09/finding-duplicate-data-with-soql/
twitterCardType:
  - summary
categories:
  - Salesforce
excerpt: "Easy to remember query to identify duplicate data in a field."
---

While the use of [Data.com Clean](https://help.salesforce.com/articleView?id=data_dot_com_clean_implementing.htm) makes it easier to ensure data quality does not get compromised, there are still times that I need to quickly check to see if I need to merge records together. For those who haven’t needed to do this before, you may not be aware that a SOQL query can do this for you (no need for Excel).

One scenario I recently went through was checking for duplicate Contact records that users may have created prior to an integration pushing the same Contact’s master data into Salesforce. The first thing I checked in this case was for unique email addresses.

    SELECT Email, COUNT(Id) FROM Contact GROUP BY Email HAVING COUNT(Id) > 1
This [Aggregate](https://developer.salesforce.com/docs/atlas.en-us.soql_sosl.meta/soql_sosl/sforce_api_calls_soql_select_agg_functions.htm) query groups together all of the resulting records by the **GROUP BY** field, in this case Email, and displays the number of Contact records that match that Email. The **HAVING** clause filters out any unique Emails in the system, ensuring that the result set only shows duplicates.

While this query doesn’t give you the Contact Ids, it is a great way to quickly narrow down the faulty data for more specific queries. Similarly, you can check other fields by modifying the **GROUP BY** attribute (and updating that value in the select attributes).
