---
id: 971
title: 'Salesforce Communities &#038; the Live Agent Deployment API'
date: 2014-08-11T18:30:58+00:00
author: Michael Welburn
layout: post
guid: http://michaelwelburn.com/?p=971
permalink: /2014/08/11/salesforce-communities-liveagent-deployment-api/
twitterCardType:
  - summary
cardImgSize:
  - small
cardImageWidth:
  - 280
cardImageHeight:
  - 150
categories:
  - Salesforce
tags:
  - Live Agent
---
<div style="color: #222222;">
  Integrating <a title="LiveAgent" href="https://www.ladesk.com/" target="_blank">Live Agent</a> with Salesforce is exceptionally easy once you&#8217;ve purchased the licenses and followed the <a title="Set up Live Agent" href="https://help.salesforce.com/apex/HTViewHelpDoc?id=live_agent_setting_up.htm" target="_blank">basic setup instructions</a>. <a title="@PeterKnolle" href="https://twitter.com/PeterKnolle" target="_blank">Peter Knolle</a> wrote a <a title="LiveAgent Pre-Chat API" href="http://peterknolle.com/live-agent-pre-chat-api/" target="_blank">blog post earlier this year</a> that outlined some of the basics around setup, as well as the usage of the Pre-Chat API that Live Agent offers. However, while working with the Deployment API, I found myself in a pickle trying to figure out why my attempts to customize part of the Service Agent&#8217;s experience weren&#8217;t working.
</div>

<p style="color: #222222;">
  <!--more-->
</p>

<div style="color: #222222;">
  Live Agent for Salesforce has two separate APIs, documented in the <a title="Live Agent Developer's Guide" href="http://www.salesforce.com/us/developer/docs/live_agent_dev/index.htm" target="_blank">Live Agent Developer&#8217;s Guide</a>: Pre-Chat & Deployment. Pre-Chat allows you to set up a form that a user might fill out prior to connecting with an agent to chat, and then leverage that information to surface content to the Agent or to resolve who the Agent is speaking to. The Deployment API does not rely on a form for the user to fill out; instead, it lets you use Javascript to populate the same content behind the scenes. This is particularly useful when you have enabled Live Agent on an authenticated website where you already know who is making the chat request.
</div>

<div style="color: #222222;">
</div>

<div style="color: #222222;">
  To leverage Live Agent in an authenticated Salesforce Community, there are two main features that I think are required to implement for your Agents.
</div>

<div>
  <ol>
    <li>
      <span style="color: #222222; font-family: 'Helvetica Neue', Helvetica, Arial, sans-serif;">Setting the name of the user initiating the chat. This will show in both the screen pop to your agent, as well as in the Chat Transcript.</span>
    </li>
    <li>
      Popping open the Community User&#8217;s related Contact record, in order to view their metadata, case history, etc.
    </li>
  </ol>
  
  <p>
    After pasting the generated Live Agent code into your Visualforce page (which we&#8217;ll use as an example), you have the ability to leverage a couple of methods off the <strong>liveagent</strong> javascript object (which are documented in the Deployment API). The easiest one to use is the <a title="setName" href="https://developer.salesforce.com/docs/atlas.en-us.live_agent_dev.meta/live_agent_dev/live_agent_customizing_visitor_details_API_setName.htm" target="_blank"><strong>setName()</strong></a> method, which will accomplish the first item mentioned above. This is particularly important because otherwise your Agents will be presented with the IP Address of the user, along with the very generic label of <strong>Visitor</strong> in the chat window. With Visualforce & merge fields, this is simply an exercise of dumping the User&#8217;s name into the method as a parameter:
  </p>
  
  <pre><code>liveagent.setName( '{!$User.FirstName} {!$User.LastName}' );</code></pre>
</div>

<div style="color: #222222;">
  Where I had some issues was with the second. I found that there were a <a title="Live Agent Deployment Code Sample" href="http://www-demo.sfdro.com/us/developer/docs/live_agent_dev/Content/live_agent_deployment_api_code_sample.htm" target="_blank">few</a> <a title="Live Agent Creating Records Sample" href="http://www.salesforce.com/us/developer/docs/live_agent_dev/Content/live_agent_creating_records_code_sample.htm" target="_blank">examples</a> of how to use the Deployment API for the <a title="findOrCreate" href="https://developer.salesforce.com/docs/atlas.en-us.live_agent_dev.meta/live_agent_dev/live_agent_creating_records_API_findOrCreate.htm" target="_blank"><strong>findOrCreate()</strong></a> method, but there was not much in terms of instructions. The gist of what this method is doing is that you can provide some query criteria for a particular object type, and it will either find a matching record or create a new one (if necessary). Since we are querying for authenticated Users in the system (that have associated Contact records), there is no need to leverage the creation aspect of the method, as we should always be able to find the record by the ContactId field on the User.
</div>

<div style="color: #222222;">
</div>

<div style="color: #222222;">
  Unfortunately, what I didn&#8217;t realize was that when using the <a title="map" href="https://developer.salesforce.com/docs/atlas.en-us.live_agent_dev.meta/live_agent_dev/live_agent_creating_records_API_map.htm" target="_blank"><strong>map()</strong></a> method is looking for a label corresponding to the value, rather than the value itself for the <strong>DetailName</strong>. What you need to do is define a custom detail, and then reference that in your map.
</div>

    liveagent.addCustomDetail('<strong>Contact ID</strong>', '{!$User.ContactId}', false);
    
    liveagent.findOrCreate('Contact').map('Id', '<strong>Contact ID</strong>', true, true, false).saveToTranscript('contactId').showOnCreate();
    

<div>
  Looking back on the aforementioned documentation after discovering this (both on the Deployment API as well as the Pre-Chat API), it made a lot more sense. I was so focused on assuming the actual value would be the parameter that I misread what the parameter&#8217;s description actually said: <em>&#8220;The value of the custom detail to map to the corresponding field FieldName&#8221;</em>. Hopefully anyone else running into the same issue will find this enlightening, as the <a title="enableLogging" href="https://developer.salesforce.com/docs/atlas.en-us.live_agent_dev.meta/live_agent_dev/live_agent_logging_API_enableLogging.htm" target="_blank">debugging tools</a> for Live Agent&#8217;s Deployment API was not very helpful for me.
</div>