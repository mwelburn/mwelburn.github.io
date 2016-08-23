---
id: 994
title: Discovering Salesforce ContentDocumentLink ShareType Picklist Values
date: 2014-08-22T08:00:11+00:00
author: Michael Welburn
guid: http://michaelwelburn.com/?p=994
permalink: /2014/08/22/discovering-salesforce-contentdocumentlink-sharetype-picklist-values/
twitterCardType:
  - summary
cardImgSize:
  - small
cardImageWidth:
  - 280
cardImageHeight:
  - 150
categories:
  - Apex
  - Programming
  - Salesforce
tags:
  - Dynamic Apex
---
I recently was implementing some Apex code that required me to share Files with Users behind the scenes, which led me to the <a title="ContentDocumentLink" href="https://www.salesforce.com/us/developer/docs/api/Content/sforce_api_objects_contentdocumentlink.htm" target="_blank">ContentDocumentLink</a> object. It seemed simple enough, and I had the option of a few different **ShareType** values to denote the level of access I was giving the Users:

  * Viewer
  * Collaborator
  * Inferred

Unfortunately, it didn&#8217;t end up being quite that simple.

<!--more-->

I thought I had set all the fields up, similar to the following example code, however I found myself receiving a strange error.

    ContentDocumentLink share = new ContentDocumentLink();
    share.ContentDocumentId = documentId;
    share.LinkedEntityId = userId;
    share.ShareType = 'Viewer';
    insert share;

> caused by: System.DmlException: Insert failed. First exception on row 0; first error: INVALID\_OR\_NULL\_FOR\_RESTRICTED_PICKLIST, Share Type: bad value for restricted picklist field: Viewer: [ShareType]

It turns out that the **ShareType** picklist values are not the values noted in the documentation, but instead are just the first (capital) letters of those values (V, C, or I). I found this out by using <a title="Using Dynamic Apex to retrieve Picklist Values" href="https://developer.salesforce.com/blogs/developer-relations/2008/12/using-the-metadata-api-to-retrieve-picklist-values.html" target="_blank">Dynamic Apex to retrieve the picklist values</a> via <a title="Executing Anonymous Apex Code" href="https://help.salesforce.com/apex/HTViewHelpDoc?id=code_dev_console_execute_anonymous.htm&language=en" target="_blank">Anonymous Apex</a> and checking the log it spit out. The snippet of code you need to run to accomplish that is simply:

    <strong>System.debug(ContentDocumentLink.ShareType.getDescribe().getPicklistValues());</strong>

And the output log is the following:

    30.0 APEX_CODE,DEBUG;APEX_PROFILING,DEBUG;CALLOUT,INFO;DB,INFO;VALIDATION,INFO;WORKFLOW,INFO
    Execute Anonymous: <strong>System.debug(ContentDocumentLink.ShareType.getDescribe().getPicklistValues());</strong>
    09:21:56.029 (29158075)|EXECUTION_STARTED
    09:21:56.029 (29169877)|CODE_UNIT_STARTED|[EXTERNAL]|execute_anonymous_apex
    09:21:56.031 (31520528)|USER_DEBUG|[1]|<strong>DEBUG|(Schema.PicklistEntry[getLabel=Viewer;getValue=V;isActive=true;isDefaultValue=false;], Schema.PicklistEntry[getLabel=Collaborator;getValue=C;isActive=true;isDefaultValue=false;], Schema.PicklistEntry[getLabel=Inferred;getValue=I;isActive=true;isDefaultValue=false;])</strong>
    09:21:56.045 (31578556)|CUMULATIVE_LIMIT_USAGE
    09:21:56.045|LIMIT_USAGE_FOR_NS|(default)|
    Number of SOQL queries: 0 out of 100
    Number of query rows: 0 out of 50000
    Number of SOSL queries: 0 out of 20
    Number of DML statements: 0 out of 150
    Number of DML rows: 0 out of 10000
    Maximum CPU time: 0 out of 10000
    Maximum heap size: 0 out of 6000000
    Number of callouts: 0 out of 10
    Number of Email Invocations: 0 out of 10
    Number of future calls: 0 out of 10
    Number of Mobile Apex push calls: 0 out of 10
    09:21:56.045|CUMULATIVE_LIMIT_USAGE_END
    09:21:56.031 (31610936)|CODE_UNIT_FINISHED|execute_anonymous_apex
    09:21:56.033 (33752316)|EXECUTION_FINISHED
