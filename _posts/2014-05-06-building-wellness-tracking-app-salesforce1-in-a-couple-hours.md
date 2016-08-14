---
id: 871
title: Building a Wellness Tracking App on Salesforce1 in a Couple Hours
date: 2014-05-06T07:00:15+00:00
author: Michael Welburn
layout: post
guid: http://michaelwelburn.com/?p=871
permalink: /2014/05/06/building-wellness-tracking-app-salesforce1-in-a-couple-hours/
categories:
  - Apex
  - Javascript
  - Programming
  - Salesforce
tags:
  - AngularJS
  - Apex
  - mobile
  - Ratchet
  - Salesforce1
  - Visualforce
---
A couple weeks back an internal team at <a title="7Summits" href="http://7summitsagency.com" target="_blank">7Summits</a> built a wellness tracking mobile application to help our staff compete against each other in a variety of wellness related challenges. That spurred me to think about how quickly the <a title="Salesforce1" href="http://www.salesforce.com/salesforce1/" target="_blank">Salesforce1 platform</a> could be used to jumpstart something similar. While I don&#8217;t have the UX and UI chops of my teammates, I did have the power of Salesforce1 to help provide the infrastructure so I had the data model up and running in minutes, and was able to jump right into the fun stuff.

<blockquote class="twitter-tweet" lang="en">
  <p>
    Tracking wellness with <a href="https://twitter.com/salesforce1">@salesforce1</a>. Built in hours, not weeks! <a href="https://twitter.com/search?q=%23salesforce1selfie&src=hash">#salesforce1selfie</a> <a href="https://twitter.com/search?q=%23sf1devweek&src=hash">#sf1devweek</a> <a href="https://twitter.com/Benioff">@Benioff</a> <a href="https://twitter.com/adamse">@adamse</a> <a href="http://t.co/5vBeBNtnQE">pic.twitter.com/5vBeBNtnQE</a>
  </p>
  
  <p>
    — Michael Welburn (@MichaelWelburn) <a href="https://twitter.com/MichaelWelburn/statuses/462381751588114432">May 3, 2014</a>
  </p>
</blockquote>

<!--more-->

## Basic Setup

This is where Salesforce really shines. Once you have an idea, it&#8217;s extremely easy to create a couple objects and fields and be ready to jump right into coding. Not having to deal with authentication, security, APIs, administrative UI, etc. is quite a blessing. After creating two objects &#8212; **Wellness Goals** to define goals and **Wellness Activities** to track User goal achievements &#8212; I was ready to fire up <a title="mavensmate" href="http://mavensmate.com/" target="_blank">mavensmate</a> and create some Visualforce pages. [<img class="aligncenter wp-image-887 size-full" src="http://michaelwelburn.com/wp-content/uploads/2014/05/Screen-Shot-2014-05-05-at-6.10.52-PM.png" alt="Screen Shot 2014-05-05 at 6.10.52 PM" width="628" height="273" srcset="http://michaelwelburn.com/wp-content/uploads/2014/05/Screen-Shot-2014-05-05-at-6.10.52-PM.png 628w, http://michaelwelburn.com/wp-content/uploads/2014/05/Screen-Shot-2014-05-05-at-6.10.52-PM-300x130.png 300w" sizes="(max-width: 628px) 100vw, 628px" />](http://michaelwelburn.com/wp-content/uploads/2014/05/Screen-Shot-2014-05-05-at-6.10.52-PM.png) I&#8217;ve been trying to play around with AngularJS on Visualforce lately, and this quick application was a great excuse to keep up that effort. Additionally, I decided to dabble with <a title="Visualforce Remote Objects" href="https://developer.salesforce.com/releases/release/Spring14/Visualforce+Remote+Objects" target="_blank">Visualforce remote objects</a>, a Spring 14 addition to the platform, explained well in this <a title="Visualforce Remote Object Introduction" href="http://andyinthecloud.com/2014/01/22/spring14-visualforce-remote-objects-introduction/" target="_blank">blog post</a> by Andrew Fawcett. This project also gave me a chance to try out <a title="Ratchet" href="http://goratchet.com/" target="_blank">Ratchet</a>, which is a framework that helps build mobile applications (and also meant I wrote zero lines of CSS).

## Logging Goals

[<img class="alignnone size-medium wp-image-902" src="http://michaelwelburn.com/wp-content/uploads/2014/05/photo-31-169x300.png" alt="photo 3" width="169" height="300" srcset="http://michaelwelburn.com/wp-content/uploads/2014/05/photo-31-169x300.png 169w, http://michaelwelburn.com/wp-content/uploads/2014/05/photo-31-576x1024.png 576w, http://michaelwelburn.com/wp-content/uploads/2014/05/photo-31.png 640w" sizes="(max-width: 169px) 100vw, 169px" />](http://michaelwelburn.com/wp-content/uploads/2014/05/photo-31.png)  [<img class="alignnone wp-image-875 size-medium" src="http://michaelwelburn.com/wp-content/uploads/2014/05/photo-1-169x300.png" alt="Searchable List of Goals" width="169" height="300" srcset="http://michaelwelburn.com/wp-content/uploads/2014/05/photo-1-169x300.png 169w, http://michaelwelburn.com/wp-content/uploads/2014/05/photo-1-576x1024.png 576w, http://michaelwelburn.com/wp-content/uploads/2014/05/photo-1.png 640w" sizes="(max-width: 169px) 100vw, 169px" />](http://michaelwelburn.com/wp-content/uploads/2014/05/photo-1.png)  [<img class="size-medium wp-image-876" src="http://michaelwelburn.com/wp-content/uploads/2014/05/photo-2-169x300.png" alt="Ability to complete a goal" width="169" height="300" srcset="http://michaelwelburn.com/wp-content/uploads/2014/05/photo-2-169x300.png 169w, http://michaelwelburn.com/wp-content/uploads/2014/05/photo-2-576x1024.png 576w, http://michaelwelburn.com/wp-content/uploads/2014/05/photo-2.png 640w" sizes="(max-width: 169px) 100vw, 169px" />](http://michaelwelburn.com/wp-content/uploads/2014/05/photo-2.png)

Defining Wellness Goals in this particular instance is something that an Administrator would do via the normal Salesforce object tab. Logging Wellness Activities, however, is something that people very likely would be doing on the go. For that reason, I decided to leverage a global <a title="Publisher Action overview" href="http://help.salesforce.com/HTViewHelpDoc?id=actions_overview.htm&language=en_US" target="_blank">publisher action</a> to bring up a custom Visualforce page.

This Visualforce page is merely retrieving a list of Wellness Goals in the system via query and displaying them on the page. I found that using remote objects was surprisingly painless, and actually was easier for simple integration with a client side javascript framework than using remote actions. It doesn&#8217;t hurt that it means I didn&#8217;t need to write unit tests. 

I don&#8217;t want to dive too much into the nuts and bolts of how remote objects work, as the blog I mentioned above does a very good job of detailing that. However, I&#8217;ll touch on some sample code below.

To use remote objects, you use the <a title="apex:remoteobjects" href="http://www.salesforce.com/us/developer/docs/pages/Content/pages_compref_remoteObjects.htm" target="_blank"><apex:remoteObjects></a> tag to define a namespace for your javascript references to sObjects, and then create child <a title="apex:remoteobjectmodel" href="http://www.salesforce.com/us/developer/docs/pages/Content/pages_compref_remoteObjectModel.htm" target="_blank"><apex:remoteObjectModel></a> elements to reference each sObject you plan to use, along with the fields. Once that is defined, in your javascript you have a variety of methods available for querying and CRUD operations.



The search functionality is courtesy of an <a title="Filter" href="https://docs.angularjs.org/api/ng/filter/filter" target="_blank">AngularJS filter</a> on the repeating list of query results, and clicking on a result pops open a modal window that lets you confirm you activity (which was a built in component with Ratchet called <a title="Ratchet modals" href="http://goratchet.com/components/#modals" target="_blank">modals</a>). Once you are ready to create a new record in the system (and have already set up your remote object definition), it is as easy as the following snippet:



One of the interesting things I&#8217;ll point out is that when I write javascript heavy pages, I like to extract out the javascript into a static resource. Unfortunately, doing this means you no longer have access to any Visualforce merge fields to get information like who the current user is. While I could just assume that whoever creates the record is the user the Wellness Activity is logged to (or implement a trigger to fill that value out if it is blank), I took a shortcut and put a snippet of javascript on my Visualforce page (prior to including my javascript file) that set **var userId = &#8220;{!$User.Id}&#8221;**. This gives me access to that value later when I need it (and is a tool you can use to grab any other Visualforce generated information as well, like standard controller field values).

<h2 style="text-align: left;">
  Building the Leaderboard
</h2>

[<img class="size-medium wp-image-878" src="http://michaelwelburn.com/wp-content/uploads/2014/05/photo-4-169x300.png" alt="Mobile Tab" width="169" height="300" srcset="http://michaelwelburn.com/wp-content/uploads/2014/05/photo-4-169x300.png 169w, http://michaelwelburn.com/wp-content/uploads/2014/05/photo-4-576x1024.png 576w, http://michaelwelburn.com/wp-content/uploads/2014/05/photo-4.png 640w" sizes="(max-width: 169px) 100vw, 169px" />](http://michaelwelburn.com/wp-content/uploads/2014/05/photo-4.png)  [<img class="size-medium wp-image-879" src="http://michaelwelburn.com/wp-content/uploads/2014/05/photo-5-169x300.png" alt="Leaderboard" width="169" height="300" srcset="http://michaelwelburn.com/wp-content/uploads/2014/05/photo-5-169x300.png 169w, http://michaelwelburn.com/wp-content/uploads/2014/05/photo-5-576x1024.png 576w, http://michaelwelburn.com/wp-content/uploads/2014/05/photo-5.png 640w" sizes="(max-width: 169px) 100vw, 169px" />](http://michaelwelburn.com/wp-content/uploads/2014/05/photo-5.png)

The leaderboard was built by taking the Wellness Activity objects logged and running an aggregate query on them, grouping by the User and limiting the results to a certain time period. Because I&#8217;m not quite sure whether aggregate queries will won&#8217;t with remote objects, I just created a RemoteAction for it.

## Summary

As I mentioned in a tweet I sent out with these screenshots on Friday, whipping together this application only took a couple hours of toying around. It&#8217;s pretty amazing that no infrastructure changes were really necessary (besides setting up the object model), and that you can dive right into coding if you want. Alternatively, you could use the native UI to accomplish the same goals in the mobile application, but I like to think this is an easier way of visualizing this particular experience.

To take a closer look at the code, I uploaded the repository to GitHub at <a title="Salesforce1 Wellness Tracker" href="https://github.com/mwelburn/Salesforce1-Wellness-Tracker" target="_blank">https://github.com/mwelburn/Salesforce1-Wellness-Tracker</a>.