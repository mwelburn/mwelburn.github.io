---
id: 21
title: 'iOS Development with Salesforce.com SDK: Day 1 thoughts'
date: 2013-02-25T08:00:23+00:00
author: Michael Welburn
layout: post
guid: http://michaelwelburn.com/?p=21
permalink: /2013/02/25/ios-development-with-salesforce-com-sdk-day-1-thoughts/
tumblr_br0nc080_permalink:
  - http://br0nc080.tumblr.com/post/43110411672/ios-development-with-salesforce-com-sdk-day-1-thoughts
tumblr_br0nc080_id:
  - 43110411672
categories:
  - iOS
  - Programming
  - Salesforce
tags:
  - coredata
  - Force.com
  - iOS
  - iPad
  - mobile
  - REST
  - Salesforce
  - SDK
  - SmartStore
  - touch
---
I finally began my first proof of concept leveraging a native iOS iPad application recently, which began with my attempts to recall basic pieces of architecture that were discussed early in the <a title="Big Nerd Ranch Guide" href="http://www.amazon.com/iOS-Programming-Ranch-Guide-Guides/dp/0321821521" target="_blank">Big Nerd Ranch Guide</a>…that I read 3 months ago. After scouring some of the example projects from the book, I had finally cobbled together what I needed to do to set up my basic master detail view. It was a slow start that made me question how productive this effort was going to be…

The overall goal of the single day effort was to take a look at <a title="Salesforce.com Touch" href="http://wiki.developerforce.com/page/Salesforce_touch_platform" target="_blank">Salesforce Touch</a>, and more specifically the <a title="Salesforce.com iOS SDK" href="http://wiki.developerforce.com/page/Getting_Started_with_the_Mobile_SDK_for_iOS" target="_blank">Salesforce.com iOS SDK</a>, and see what kind of functionality it provided and how easy it was to use. I had a couple particular goals that I was taking a look at more closely.

<img class="size-full wp-image-145 aligncenter" alt="tumblr_inline_mi8q18JNYV1qz4rgp" src="http://michaelwelburn.com/wp-content/uploads/2013/02/tumblr_inline_mi8q18JNYV1qz4rgp.png" width="500" height="387" srcset="http://michaelwelburn.com/wp-content/uploads/2013/02/tumblr_inline_mi8q18JNYV1qz4rgp.png 500w, http://michaelwelburn.com/wp-content/uploads/2013/02/tumblr_inline_mi8q18JNYV1qz4rgp-300x232.png 300w" sizes="(max-width: 500px) 100vw, 500px" />

<!--more-->

**Authentication**

I was pleasantly surprised when I fired up the basic native Salesforce.com project and ran it the first time, as the authentication mechanism was already completely built out leveraging OAuth. The only modification I ended up making was adding a button to logout in order to test different users, and even then it was just a matter of referencing the logout method from the AppDelegate from an action in my list view.

    - (void)logout:(id)sender
    {
       AppDelegate *del = (AppDelegate *)[[UIApplication sharedApplication] delegate];
       [del logout];
    }

<p class="p1">
  <img class="alignnone size-full wp-image-144 aligncenter" alt="tumblr_inline_mi8q1qYMbt1qz4rgp" src="http://michaelwelburn.com/wp-content/uploads/2013/02/tumblr_inline_mi8q1qYMbt1qz4rgp.png" width="500" height="387" srcset="http://michaelwelburn.com/wp-content/uploads/2013/02/tumblr_inline_mi8q1qYMbt1qz4rgp.png 500w, http://michaelwelburn.com/wp-content/uploads/2013/02/tumblr_inline_mi8q1qYMbt1qz4rgp-300x232.png 300w" sizes="(max-width: 500px) 100vw, 500px" />
</p>

**REST API**

The <a title="REST API" href="http://wiki.developerforce.com/page/REST_API" target="_blank">REST API</a> was the piece I was most interested in. I’ve been wanting to toy with leveraging Salesforce.com data outside of the Force.com ecosystem for a while, as I’ve primarily only been customizing Apex and Visualforce. From earlier reading I had done, I knew the API allowed me to do CRUD operations against all objects, as well as run queries, retrieve system definitions, and much more. While the Force.com platform allows creation of <a title="Custom REST API" href="http://wiki.developerforce.com/page/Creating_REST_APIs_using_Apex_REST" target="_blank">custom</a> REST API endpoints (which I’ve done a couple of times), for the most part I suspected my needs would be fulfilled by the standard endpoints.

Right out of the gate, the SDK makes life incredibly easy by wrapping most of the endpoints in methods that obscure the actual building of the REST call and instead require specific parameters, for example query string to retrieve records, or an array of JSON records to upsert. Someone with no knowledge of the actual API (or even Salesforce.com) could get pretty far as long as someone provided them object definitions, which I suppose is the goal in this case — making the Salesforce.com integration easy so more time can be spent on other parts of the application.

**Offline Storage**

Offline storage is something I haven’t really worked with at all as a developer. I had read a bit and toyed with CoreData, Apple’s persistence framework, but nothing on a production level scale. Perhaps the most interesting piece of information about the Salesforce Mobile SDK’s implementation of offline storage was that they were leveraging an alternative to CoreData, rather than wrapping CoreData itself. They called this SmartStore, and it also sits on top of an SQLite database. Here is a good <a title="SmartStore" href="http://www.modelmetrics.com/tomgersic/storing-data-offline-with-salesforce-mobile-sdk-smartstore/" target="_blank">article</a> by Model Metrics about it.

While most documentation regarding SmartStore references its use for Hybrid applications, it is also available for native applications as well. That is where I spent some time trying to figure out what the use cases were for a native developer to use SmartStore rather than CoreData, considering CoreData is the standard for local storage. I found a Salesforce <a title="SmartStore Blog" href="http://blogs.developerforce.com/developer-relations/2012/11/using-smartstore-with-native-ios.html" target="_blank">blog</a> that boiled it down to using SmartStore on native applications primarily to maintain parity with any Hybrid applications, but am still curious if there are any other benefits regarding performance or any other metrics (e.g. ease of use). From my brief work leveraging it, it didn’t necessarily seem any easier than using CoreData.

**Roadblocks**

The main issue I ran into was that I was taught to build my UI using the following method:

    - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions

However, it appeared that the SDK have its own method it required that you implement to set the rootViewController:

    - (UIViewController *)newRootViewController

Even more problematic was that when I tried to set up the newRootViewController method to return a reference to a  UISplitViewController, I got an error saying “Application tried to present a Split View Controller modally”.

After looking on the web, only a couple of people complained about running into the same issue. The solution in this case was to override the following method to allow a UISplitViewController to be the root.

    - (void)setupNewRootViewController
    {
       UIViewController *rootVC = [[self newRootViewController] autorelease];
       self.viewController = rootVC;
       self.window.rootViewController = rootVC;
    }

<p class="p1">
  The other disappointing thing was that it seemed like the available API endpoints didn’t support uploading content to Salesforce.com. Per the documentation, it looked like the REST API required joining a pilot program for that access (as of Spring 13), though I couldn’t find any SDK methods that let me retrieve content from attachments either (even though that appears to be supported via the API)
</p>

<p class="p1">
  Lastly, while not an issue, the Salesforce iOS SDK doesn’t support ARC, the automatic memory management tool that is currently used (and more importantly, what all the example projects I built used). This meant that if I was to build a production application I’d have to handle all the memory management myself. Not the end of the world, but another thing to keep in mind when scoping out the amount of work that hunting down memory leaks might be part of your effort.
</p>

* * *

By the end of the day, I found myself far more comfortable than I thought I’d be. Obviously there is an enormous amount of syntax that I am completely oblivious to, but I began to have a high level of idea of what I needed to do in order to accomplish particular things, and then I’d spend some time looking up the exact syntax. While I don’t plan on doing any sort of graphics or games in the near future, the ease of quickly building something on this SDK makes me excited for building some data driven applications in the future. You can see what came out of less than a day of development from a developer working on the platform on his own the first time: an application that authenticates, retrieves metadata, allows updating fields, and has some rudimentary offline storage implemented.

The one thing about the Salesforce.com Mobile SDK that was interesting to me was how much more documentation and examples there seemed to be regarding Hybrid approaches to mobile development. It seems like they are pushing this approach more than the native applications. This can be seen in particular in the <a title="Salesforce.com Touch Platform Mobile Development Guide" href="http://media.developerforce.com/pdfs/salesforce_touch_platform.pdf" target="_blank">Salesforce.com Touch Platform Mobile Development Guide</a>, where there is multiple sections dedicated to it as opposed to the single chapter on iOS native development.