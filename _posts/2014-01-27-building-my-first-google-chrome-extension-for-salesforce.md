---
id: 744
title: Building My First Google Chrome Extension for Salesforce
date: 2014-01-27T06:00:16+00:00
author: Michael Welburn
layout: post
guid: http://michaelwelburn.com/?p=744
permalink: /2014/01/27/building-my-first-google-chrome-extension-for-salesforce/
categories:
  - Javascript
  - Salesforce
  - Side Projects
tags:
  - Chrome Extension
  - JavaScript
  - Salesforce
---
Building a Chrome Extension was never something I had considered. I&#8217;ve used plenty to enhance different applications that I use, but I never had stopped to think about repetitive tasks that I frequently do and ways to try to automate them.

A couple months ago I came across Jesse Altman&#8217;s blog post titled <a title="Useful Google Chrome Extensions for Salesforce" href="http://jessealtman.com/2013/09/useful-google-chrome-extensions-for-salesforce/" target="_blank">Useful Google Chrome Extensions for Salesforce</a>. Up until that time I hadn&#8217;t even realized that there might be a library of useful extensions that I hadn&#8217;t even looked for, and after reading through his post and searching around through the available extensions on the <a title="Chrome Web Store: Salesforce Extensions" href="https://chrome.google.com/webstore/search-extensions/salesforce" target="_blank">Chrome Web Store</a>, I decided that I wanted to try to make one myself.

<!--more-->

I started out by coming up with a pain point in my day to day work on Salesforce projects: having to work in multiple organizations in the same day, and needing to look up data. Typically the only way to do this is by opening a developer console for each of the different organizations, or keeping <a title="Force.com IDE" href="http://wiki.developerforce.com/page/Force.com_IDE" target="_blank">Eclipse</a> or <a title="MavensMate" href="http://mavensmate.com/" target="_blank">MavensMate</a> projects open for each and using their querying tools. I decided that if I created an extension that piggybacked onto the session ID that the browser leverages while signed into a Salesforce organization, I could hook into the REST API and make queries without having to go through any additional authentication.

A quick Google search for people doing something similar brought me to Wes Nolte&#8217;s post on <a title="Developing Chrome Extensions for Salesforce" href="http://th3silverlining.com/2013/09/14/developing-chrome-extensions-for-salesforce/" target="_blank">Developing Chrome Extensions for Salesforce</a>. Fortunately for me, it had a lot of the pieces that I was going to need, in particular the ability to retrieve the session ID and leveraging <a title="ForceTK" href="https://github.com/developerforce/Force.com-JavaScript-REST-Toolkit" target="_blank">ForceTK</a>, which is a Javascript Toolkit that abstracts the REST API.

The problem I wanted to solve required some user interaction, so I then went to Google&#8217;s <a title="Getting Started: Building a Chrome Extension" href="http://developer.chrome.com/extensions/getstarted.html" target="_blank">beginner documentation</a> on building Chrome extensions. This sample added a browser action (one of those buttons on the toolbar like AdBlock) that opened an HTML page that showed images. I started to take the pieces from this example and fuse them together with the snippets from what Wes had created to started putting together a way for the user to enter a query into a text area and submit it to Salesforce.

In order to make the browser action talk to Salesforce, I had to implement the HTML and JavaScript on the browser bar, and the additional JavaScript that would be injected into my page. In order to have these two pieces interact, I ended up using <a title="Messaging" href="http://developer.chrome.com/extensions/messaging.html" target="_blank">message passing</a>. This is how I was able to send a query from an HTML file that was not hosted on Salesforce (and thus didn&#8217;t have the session ID).

After the pieces were in place, I ended up with the following.

[<img class="aligncenter size-large wp-image-767" alt="force-soql-popup" src="http://michaelwelburn.com/wp-content/uploads/2014/01/force-soql-popup-1024x522.png" width="600" height="305" srcset="http://michaelwelburn.com/wp-content/uploads/2014/01/force-soql-popup-1024x522.png 1024w, http://michaelwelburn.com/wp-content/uploads/2014/01/force-soql-popup-300x153.png 300w, http://michaelwelburn.com/wp-content/uploads/2014/01/force-soql-popup-668x340.png 668w, http://michaelwelburn.com/wp-content/uploads/2014/01/force-soql-popup.png 1182w" sizes="(max-width: 600px) 100vw, 600px" />](http://michaelwelburn.com/wp-content/uploads/2014/01/force-soql-popup.png)

A few notes for beginners:

  * When you save an update of your contentscript file(s), you need to refresh the plugin in your browsers extension directory: <a href="chrome://extensions/" target="_blank">chrome://extensions/</a>
  * To <a title="Debugging" href="http://developer.chrome.com/extensions/tut_debugging.html" target="_blank">debug</a> the browser action, you need to right click on it and choose **Inspect Popup**. This will open a separate Developer Tools window for it.

I did run into a couple of minor issues along the way:

  * I started getting <a title="StackOverflow" href="http://stackoverflow.com/questions/18430879/jquery-2-0-3-chrome-error-resources-must-be-listed-in-the-web-accessible-resourc" target="_blank">warnings</a> in my developer tools console about a jQuery map file missing. I ended up having to download the file it was looking for and include it alongside the jQuery file in my resources.
  * I also got some cryptic error when attempting to run the query **SELECT Id, Name FROM Case** that I posted to the <a title="StackExchange" href="http://salesforce.stackexchange.com/questions/22979/forcetk-in-a-chrome-extension-cross-domain-query-issue" target="_blank">Salesforce StackExchange</a> site. While I thought it had something to do with a CORS issue, it turns out I was querying for the non-existent Name attribute on the Case object&#8230;

Overall, the learning experience of putting together a quick proof of concept on Chrome Extensions was enlightening. Getting something to work is pretty basic, and since it is all JavaScript there isn&#8217;t much of a ramp up (and Google&#8217;s documentation seems pretty good).

The POC itself obviously has a lot of features lacking (and bugs), but it gave me some good ideas on better ways to bring this functionality to users. I&#8217;ve actually realized that adding another tab to the Developer Tools would be a much better place to run queries (similar to how the <a title="AngularJS Batarang" href="https://chrome.google.com/webstore/detail/angularjs-batarang/ighdmehidhipcmcojjgiloacoafjmpfk?hl=en" target="_blank">AngularJS Batarang</a> extension is implemented).

If you want to take a look at the source code, I put it on <a title="GitHub" href="https://github.com/mwelburn/Chrome-Extension-Force-SOQL-Popup" target="_blank">GitHub</a>.