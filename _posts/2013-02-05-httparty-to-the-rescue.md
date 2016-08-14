---
id: 11
title: HTTParty to the Rescue
date: 2013-02-05T01:13:00+00:00
author: Michael Welburn
layout: post
guid: http://michaelwelburn.com/2013/02/05/httparty-to-the-rescue/
permalink: /2013/02/05/httparty-to-the-rescue/
tumblr_br0nc080_permalink:
  - http://br0nc080.tumblr.com/post/42316577412/httparty-to-the-rescue
tumblr_br0nc080_id:
  - 42316577412
categories:
  - Programming
  - Ruby on Rails
  - Salesforce
tags:
  - Governor Limits
  - heroku
  - httparty
  - Ruby on Rails
  - Salesforce
redirect_from:
  - /post/42316577412/httparty-to-the-rescue/
  - /post/42316577412/
---
Working with on Salesforce.com <a title="Developer Force.com" href="http://developer.force.com/" target="_blank">platform</a>, part of my job ends up revolving around finding unique ways to solve coding limitations. Salesforce.com has a myriad of <a title="Governor Limits" href="http://www.salesforce.com/us/developer/docs/apexcode/Content/apex_gov_limits.htm" target="_blank">limits</a> in place around code to prevent a single organization from hogging resources in the multitenant environment. In theory, this has actually made my code quite a bit more efficient when applying some of the principles to other platforms, but it also makes you design your applications quite a bit differently than you might have done in the past.

<!--more-->

One of the limitations of the platform only allows one level of asynchronous activity: the initial invocation of code, and the ability to create a select number of future methods that will execute in a few minutes asynchronously. Unfortunately, those asynchronous calls can’t chain together additional asynchronous calls. One of the implementations I recently did required that extra level of asynchronous activity via the following process:

  1. Receive a <a title="Webhook" href="http://en.wikipedia.org/wiki/Webhook" target="_blank">webhook</a> with information about an event that just occurred.
  2. Query the API to see if the event had been completed. If not, wait a few minutes and try again.
  3. Retrieve a list of all the users tied to the completed event.

It turns out that the only way I could find to get around this limit was to execute step 1, persist the event information in a table (in this case a custom object), and then set up a scheduled job that ran every few minutes querying the API for event completion. Another unfortunate limitation came about here: an HTTP callout via a scheduled job already counts as an asychronous call.

At this point, I needed a way to make the additional calls. What I ended up deciding to do was to fire off a request to another server (not Salesforce.com) that told it to make a subsequent request back to an endpoint with some custom parameters that would then invoke the final step.

_[Note: While writing this post, I came to the realization that I simply could have updated the completed status of the event record in the table I used for step 2 in order to invoke a new set of conditional actions via the next running of the scheduled job. Doh. At least it forced me to learn some interesting stuff below.]_

In order to do this, I needed to quickly spin up a single endpoint application that would take in a few parameters (namely the URL I wanted to redirect back to and some additional parameters about the event), parse them, and make a server-side HTTP request right back (didn’t want to mess with <a title="CORS" href="http://en.wikipedia.org/wiki/Cross-origin_resource_sharing" target="_blank">CORS</a>). I’ve had a lot of experience with Java in the past using <a title="URLConnection" href="http://docs.oracle.com/javase/6/docs/api/java/net/URLConnection.html" target="_blank">URLConnection</a> to create requests in main classes for testing, but hadn’t dealt with spinning up a simple application in a long time, so I decided to try out a quick Ruby on Rails application hosted on <a title="Heroku" href="http://www.heroku.com/" target="_blank">Heroku</a>. _[Note: this is probably a great use case for <a title="Sinatra" href="http://www.sinatrarb.com/" target="_blank">Sinatra</a>, but I haven’t tried it out yet and wanted to knock this out quick.]_

I’ve done some Ruby on Rails programming in the past, primarily fun little side projects designed more towards <a title="B2C" href="http://www.marketingterms.com/dictionary/b2c/" target="_blank">B2C</a> web applications that I never get around to finishing, but apparently I had never needed to write code for making HTTP requests out of Rails because I had no idea how (apparently I’ve been spoiled by API gems). After doing some quick research, I realized that the out of the box Ruby library <a title="Net::HTTP" href="http://www.rubyinside.com/nethttp-cheat-sheet-2940.html" target="_blank">Net::HTTP</a> seemed a bit more wordy than I wanted. Instead, I ended up leveraging <a title="HTTParty" href="https://github.com/jnunemaker/httparty" target="_blank">HTTParty</a>, a lightweight gem that abstracted what I needed to do (make a single request) into one line of code:

    response = HTTParty.get(‘http://myurl.com?param1=abc’)

I was pleasantly surprised at how simple this use case ended up being. Coding something up like this in <a title="Apex HttpRequest Class" href="http://www.salesforce.com/us/developer/docs/apexcode/Content/apex_classes_restful_http_httprequest.htm" target="_blank">Apex</a> (or Java) takes significantly more lines of code out of the box (similar to how I imagine Net::HTTP works), and it might even be worthwhile to set up a quick class in Apex to replicate this kind of logic in the future. Additionally, the Rails + Heroku solution couldn’t have taken more than 15 minutes to get live. Hooray for the cloud.
