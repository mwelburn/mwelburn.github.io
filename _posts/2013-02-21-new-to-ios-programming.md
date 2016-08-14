---
id: 22
title: New to iOS Programming
date: 2013-02-21T08:00:34+00:00
author: Michael Welburn
layout: post
guid: http://michaelwelburn.com/?p=22
permalink: /2013/02/21/new-to-ios-programming/
tumblr_br0nc080_permalink:
  - http://br0nc080.tumblr.com/post/42889949058/new-to-ios-programming
tumblr_br0nc080_id:
  - 42889949058
categories:
  - iOS
  - Programming
tags:
  - BigNerdRanch
  - blocking
  - bracketsyntax
  - dotsyntax
  - Interface Builder
  - iOS
  - iPad
  - iPhone
  - Objective-C
  - Offline Storage
  - pointers
  - tablecell
---
For a while now, I’ve wanted to learn iOS programming. It is easy to get caught up in some of the really neat applications available for iPhones and iPads and lust after wanting to design something similar (although the odds of making much money on your own applications tend to be extremely low).

Last fall I decided I would take some (a lot) of spare time and power through the <a title="Big Nerd Ranch" href="http://www.bignerdranch.com/" target="_blank">Big Nerd Ranch</a>’s <a title="iOS Programming Guide" href="http://www.amazon.com/iOS-Programming-Ranch-Edition-Guides/dp/0321821521/ref=dp_ob_title_bk" target="_blank">iOS Programming Guide</a>. At my previous employer, I had some high hopes that if I could throw together a prototype we could sell some iOS (or mobile) work and drive some interesting projects, but unfortunately I didn’t get a chance to follow through on that. After taking a month or two off concentrating on actual work (and then switching jobs), I finally got enough free time to close out the last few chapters recently in anticipation of getting to toy with <a title="Salesforce Touch" href="http://www.salesforce.com/platform/touchplatform/" target="_blank">Salesforce Touch</a>.

<!--more-->

As with any programming book, the real test is once you are making something on your own with the crutch of the actual solutions in a book to fall back on. Coming from a background that is heavily focused on Java and Javascript, there were quite a few concepts that either were pretty new to me or things that I didn’t leverage very much in the past that seem to be pretty core to how Objective-C gets leveraged.

**Pointers**

It has been a while since I’ve had to deal with <a title="Pointers" href="http://www.drdobbs.com/mobile/pointers-in-objective-c/225700236" target="_blank">pointers</a>, as I’ve been spoiled with a lot of Java, but they are quite heavily used in Objective-C. I haven’t delved too deeply into special usages at this point, as basically every instance variable that isn’t a primitive ends up being a pointer anyway.

**Bracket Syntax**

Objective-C seems to leverage brackets instead of periods to reference methods on object instances. In Java, what would be myObject.start() becomes [myObject start] in Objective-C. It appears that dot syntax is supported, but there are <a title="Dot Notation" href="http://blog.bignerdranch.com/83-83/" target="_blank">many</a> interesting <a title="Quality Coding: Dot Notation" href="http://qualitycoding.org/dot-notation/" target="_blank">ideas</a> about why it is terrible to use.

**Extremely Verbose Method Signatures**

The first thing that you’ll notice when taking a look at Objective-C code is that invoking a method with parameters means that you’ll see very explicit parameter descriptions. In Java (or other languages I’ve worked with more recently), the following method invocation of is pretty simple but hard to understand for someone unfamiliar with the code base:

    obj.watchMove(“Skyfall”, true, “Water”, false);

Whereas in Objective-C, someone completely new to the program could read this line of code and know exactly what it was doing (provided the parameters were named well).

    [obj watchMovie:@”Skyfall” whileEatingPopcorn:YES whileDrinking:@”Water” whileStanding:NO]

While this sort of bloats the amount of code you look at, it is very useful from a readability perspective.

**Interface Builder**

One of the neat things about designing an interface to an application is that you can leverage Interface Builder. Interface Builder is a UI that allows you to drag different objects onto a visual iPad or iPhone rendering and position them however you want. These objects can be as simple as a label or a text box, and they can scale up in complexity. Once these objects are in place, you then can link them to variables defined in your code, set the variables, reload your view and the values will start mapping.

<img class="size-full wp-image-149 aligncenter" alt="tumblr_inline_mi8u770FnO1qz4rgp" src="http://michaelwelburn.com/wp-content/uploads/2013/02/tumblr_inline_mi8u770FnO1qz4rgp.png" width="360" height="520" srcset="http://michaelwelburn.com/wp-content/uploads/2013/02/tumblr_inline_mi8u770FnO1qz4rgp.png 360w, http://michaelwelburn.com/wp-content/uploads/2013/02/tumblr_inline_mi8u770FnO1qz4rgp-207x300.png 207w" sizes="(max-width: 360px) 100vw, 360px" />

Another cool thing about this is that it isn’t limited to just the high level view of your entire iPhone or iPad. You can also design smaller pieces, such as how list items look. You can imagine this based on how something like the official Twitter application looks, where you can scroll down a list of items but they are formatted differently (avator, twitter handle, and tweet rather than a single line of text).

Here’s a quick example of a custom item cell from the book in Interface Builder and the end result for the user.

<img class="size-full wp-image-148 aligncenter" alt="tumblr_inline_mi8u5wU88n1qz4rgp" src="http://michaelwelburn.com/wp-content/uploads/2013/02/tumblr_inline_mi8u5wU88n1qz4rgp.png" width="357" height="80" srcset="http://michaelwelburn.com/wp-content/uploads/2013/02/tumblr_inline_mi8u5wU88n1qz4rgp.png 357w, http://michaelwelburn.com/wp-content/uploads/2013/02/tumblr_inline_mi8u5wU88n1qz4rgp-300x67.png 300w" sizes="(max-width: 357px) 100vw, 357px" />

<img class="size-full wp-image-147 aligncenter" alt="tumblr_inline_mi8u3r9smP1qz4rgp" src="http://michaelwelburn.com/wp-content/uploads/2013/02/tumblr_inline_mi8u3r9smP1qz4rgp.png" width="319" height="105" srcset="http://michaelwelburn.com/wp-content/uploads/2013/02/tumblr_inline_mi8u3r9smP1qz4rgp.png 319w, http://michaelwelburn.com/wp-content/uploads/2013/02/tumblr_inline_mi8u3r9smP1qz4rgp-300x98.png 300w" sizes="(max-width: 319px) 100vw, 319px" />

**Blocking**

I used some closures a bit with jQuery in the past, so I had some familiarity with them, but I never really leveraged them beyond the basic examples of callback functions on event handlers. After going through the chapter on blocking in the guide, it really emphasizes how much blocks are used to handle different use cases, and the fact that they can even be chained together.

The most obvious example used in the guide was defining a block of code that gets called upon completion of an asynchronous method, which is similar to the jQuery use case I mentioned above.

**Offline Storage**

One of the important parts of mobile development is not assuming that users have access to the internet all the time. In this sense, caching data or allowing users to continue working without needing to connect to a data source is critical to usability. Working on primarily on enterprise level web applications, internet access was always a requirement since usage was via a computer browser, so I’ve always worked directly against a live database. I’m pretty interested to see different ways that offline storage and synchronization get handled (behind the scenes vs user initiated) in the future.

* * *

After finishing up the guide, I’m pretty excited to get started on some sort of actual application. It is hard to imagine how long it took people to ramp up on new programming languages 15 years ago when there was nowhere near the documentation available online. With websites like <a title="StackOverflow" href="http://stackoverflow.com/" target="_blank">StackOverflow</a> that have documented all sorts of issues people have run into, it makes picking up new languages something you can do in your spare time these days.