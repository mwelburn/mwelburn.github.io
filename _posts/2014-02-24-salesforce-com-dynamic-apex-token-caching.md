---
id: 821
title: Salesforce.com Dynamic Apex Token Caching
date: 2014-02-24T07:00:20+00:00
author: Michael Welburn
layout: post
guid: http://michaelwelburn.com/?p=821
permalink: /2014/02/24/salesforce-com-dynamic-apex-token-caching/
categories:
  - Apex
  - Salesforce
tags:
  - Dynamic Apex
---
I&#8217;ve started leveraging <a title="Dynamic SOQL" href="http://www.salesforce.com/us/developer/docs/apexcode/Content/apex_dynamic_soql.htm" target="_blank">Dynamic SOQL</a> for some different projects, one of which involved building a lighter weight version of the out of the box Workflow Rule Criteria that Salesforce provides, essentially allowing people to build queries via an interface that has picklist values to select a field and an expected value. My goal in this case was to cache as much of the <a title="Dynamic Apex" href="http://www.salesforce.com/us/developer/docs/apexcode/Content/apex_dynamic.htm" target="_blank">Dynamic Apex</a> schema describes as possible to prevent handle to do any additional work when parts of the page rerendered.

<!--more-->

Dynamic Apex, as I&#8217;ve <a title="Salesforce.com Dynamic Apex Field Manipulation" href="http://michaelwelburn.com/2014/01/20/salesforce-com-dynamic-apex-field-manipulation/" target="_blank">talked about in the past briefly</a>, is an incredibly powerful way to write Apex code that is completely independent of referencing fields in your organization. You can retrieve all the objects in your org, all the fields on those objects, field types, etc. This is particularly useful for ISVs who have to design a product for any number of potential customers without actually being able to see into their organization. Matt Lacey had an interesting note in one of his <a title="Developing as an ISV partner" href="http://www.laceysnr.com/2014/01/developing-as-isv-partner-one-year-down.html" target="_blank">blog posts</a> a month back referencing optional features, and the headache that it causes ISVs when they have to write code to determine whether things like record types are enabled, if person accounts are turned on, etc.

Getting back onto the original subject of this post&#8230;in this particular case, the snippet of source code below for tracking each criteria on the page attempted to cache the <a title="Schema.DescribeFieldResult" href="http://www.salesforce.com/us/developer/docs/dbcom_apex250/Content/apex_methods_system_fields_describe.htm" target="_blank">Schema.DescribeFieldResult</a> for the field. You can see that the methods that return information about the field (that leverage the Dynamic Object information) leverage a getDescribeResult() method on the class that only has to make the describe call a single time, and then caches it to a variable. In practice, I wrote a number of helper methods to retrieve valuable information about the field to determine what the interface would render.



Unfortunately, I started running into the following error every time I invoked my private getDescribe() method and then rerendered the page.

    System.SerializationException: Not Serializable: Schema.DescribeFieldResult

Quickly I realized my mistake; Describe results are inherently <a title="Dynamic Objects" href="http://www.salesforce.com/us/developer/docs/apexcode/Content/apex_dynamic_describe_objects_understanding.htm" target="_blank">not serializable</a>&nbsp;(even though I declared it privately). If I wanted to cache that value, I&#8217;d have to persist the token (which ends up being the value that I call getDescribe() on.

In this case, the <a title="sObjectField" href="http://www.salesforce.com/us/developer/docs/apexcode/Content/apex_class_Schema_SObjectField.htm" target="_blank">Schema.sObjectField</a> is serializable, so I can keep a copy of that and prevent having to make repetitive schema calls, and then I can simply use that reference to update the Schema.DescribeFieldResult. Taking things a step further, it would be even better to move a portion of that implementation a level up, so the describe for the object only had to happen once for all the child criteria.



The alternative in this case is to keep the variable as is, but apply the <a title="Transient" href="http://www.salesforce.com/us/developer/docs/apexcode/Content/apex_classes_keywords_transient.htm" target="_blank">transient</a> keyword to the declaration. Transient implies that the data in that variable does not need to be saved, and thus is not part of the view state. This means it resets each time the page is rendered, which unfortunately would mean less caching would happen, but part of my particular implementation involved making a lot of references to the describe result at once, so it would end up caching results for each individual load.

In order to leverage the transient keyword, you simply change the declaration in the above example to:

    @TestVisible private transient Schema.DescribeFieldResult describeResult;