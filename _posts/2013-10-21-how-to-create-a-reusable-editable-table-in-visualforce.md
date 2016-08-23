---
id: 595
title: How to Create a Reusable Editable Table in Visualforce
date: 2013-10-21T07:00:31+00:00
author: Michael Welburn
guid: http://michaelwelburn.com/?p=595
permalink: /2013/10/21/how-to-create-a-reusable-editable-table-in-visualforce/
categories:
  - Programming
  - Salesforce
tags:
  - edit multiple records
  - GitHub
  - Visualforce
---
One of the most frequent complaints that I hear from existing Salesforce users is the inability to quickly add, edit, or delete a group of records. The are a variety of use cases that meet this criteria, but an example of this is if a user was creating a brand new Account and had the business cards of 5 people. In this scenario, once the Account was created, the user would have to click &#8220;New Contact&#8221; 5 separate times, even if the user only knew a small bit of information about each Contact. The same is true for both editing as well as deletion.

Unfortunately, the greater the number of child records that need to be added, the more painful this method becomes. By that observation, the quantity of data that your team is trying to put into Salesforce directly affects how poor their user experience is going to be. However, when there is a repeatable manual process that is taking too much time, there is also an opportunity for code to simply the task into one screen.

<p style="text-align: center;">
  <img class="aligncenter  wp-image-597" alt="View - Editing Records" src="http://michaelwelburn.com/wp-content/uploads/2013/10/View-Editing-Records.png" width="600" height="273" srcset="http://michaelwelburn.com/wp-content/uploads/2013/10/View-Editing-Records.png 806w, http://michaelwelburn.com/wp-content/uploads/2013/10/View-Editing-Records-300x136.png 300w, http://michaelwelburn.com/wp-content/uploads/2013/10/View-Editing-Records-668x304.png 668w" sizes="(max-width: 600px) 100vw, 600px" />
</p>

<p style="text-align: left;">
  <!--more-->
</p>

The answer to this is creating a reasonably simple custom <a title="http://mwelburn.github.io/Visualforce-Edit-Multiple-Records/" href="http://mwelburn.github.io/Visualforce-Edit-Multiple-Records/" target="_blank">Visualforce page that shows a table of records</a>, allowing the user to add/remove/edit child records (in the example above, Contacts against an Account) all from a single page. While I&#8217;ve never gathered any hard data about time saved when using these pages, there is usually quite a bit of excitement around saving those pockets of minute here and there. In fact, for some customers, the idea of _only_ adding 5 child records would be a dream &#8212; they might be adding anywhere from 25 to 50 at a time.

After implementing this request a handful of times, specific to each customers use case, I thought it would be an interesting side project to take my knowledge of the solution and try to make it as reusable as possible. When working on smaller client requests, I don&#8217;t have quite as much of an opportunity to play with trying to create reusable components. As such, my goal was to try and create the most reusable implementation possible that only referenced <a title="http://www.salesforce.com/us/developer/docs/apexcode/Content/langCon_apex_SObjects.htm" href="http://www.salesforce.com/us/developer/docs/apexcode/Content/langCon_apex_SObjects.htm" target="_blank">sObjects</a>, and then implement a class that extended that and added only what was necessary to get the implementation to work (essentially any hard references to fields of the actual object type).

One of the interesting things I did have to figure out was how to actually test an abstract class &#8212; amazingly, it was something that I hadn&#8217;t actually needed to implement in the past. Abstract classes are not allowed to be explicitly instantiated as objects (which is why I chose to use it, the class would not work well on its own). As such, I ended up having to create a mock class that extended the abstract class in my unit test that implemented a constructor in order to invoke all the helper methods.

If you&#8217;d like to dive in a bit more, take a look <a title="http://mwelburn.github.io/Visualforce-Edit-Multiple-Records/" href="http://mwelburn.github.io/Visualforce-Edit-Multiple-Records/" target="_blank">here</a> at the GitHub page I set up. At the top of that page, it gives you the option to download the source, install the solution to your organization, or get redirected to the actual <a title="https://github.com/mwelburn/Visualforce-Edit-Multiple-Records" href="https://github.com/mwelburn/Visualforce-Edit-Multiple-Records" target="_blank">GitHub repository</a>. Below, it shows some screenshots and talks a little bit about what it would take to set it up. Between those 3 options, you should be able to play around with the source code as is, and then start manipulating it to fit your particular needs.

One of my hopes by making this source code public is to try and get some input from other Salesforce developers as to whether there are any other best practices that I am neglecting. To this point, most of what I&#8217;ve put together lacks a second set of trained eyes, so I&#8217;m hoping that some new tidbits of knowledge find their way over.
