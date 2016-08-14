---
id: 549
title: Visualforce PDF Table Cells and Mysterious Disappearing Borders
date: 2013-10-15T09:00:56+00:00
author: Michael Welburn
layout: post
guid: http://michaelwelburn.com/?p=549
permalink: /2013/10/15/visualforce-pdf-table-cells-and-mysterious-disappearing-borders/
categories:
  - Programming
  - Salesforce
tags:
  - css
  - pdf
  - Salesforce
  - Visualforce
---
One of the coolest features of Visualforce (in my opinion) is the ability to render your page as a PDF. This has numerous use cases (including creating [custom Visualforce Quotes](http://michaelwelburn.com/2013/03/18/sending-visualforce-pages-as-email-attachments-in-salesforce/ "Sending Visualforce Pages as Email Attachments in Salesforce")), but the one that I see the most is trying to surface a bunch of custom data on a one sheet page for printing and/or emailing to customers.

<!--more-->

One of the issues I&#8217;ve run into quite a bit is that the PDF output for table cells just doesn&#8217;t look right. You can see below what my HTML table looks like:

[<img class="aligncenter" alt="Screen Shot 2013-10-08 at 12.14.02 PM" src="http://michaelwelburn.com/wp-content/uploads/2013/10/Screen-Shot-2013-10-08-at-12.14.02-PM.png" width="686" height="114" />](http://michaelwelburn.com/wp-content/uploads/2013/10/Screen-Shot-2013-10-08-at-12.14.02-PM.png)

And then you can see, based on what you think is valid HTML/CSS, what its PDF rendering looks like.

[<img class="aligncenter" alt="Screen Shot 2013-10-08 at 12.13.43 PM" src="http://michaelwelburn.com/wp-content/uploads/2013/10/Screen-Shot-2013-10-08-at-12.13.43-PM.png" width="507" height="120" />](http://michaelwelburn.com/wp-content/uploads/2013/10/Screen-Shot-2013-10-08-at-12.13.43-PM.png)

As you can see, some of the cell borders seem to be missing, although it seems to be in random locations. However, it isn&#8217;t truly random, as you are able to reload the page and see the same visual artifacts.

> ## Developer Tip
> 
> When debugging PDF markup, set your renderAs attribute on the <apex:page> to {!$CurrentPage.parameters.renderAs} and then add renderAs=pdf as a parameter in your URL. It will let you quickly remove that parameter to switch back and forth between HTML and PDF modes (since you can&#8217;t take advantage of browser based developer tools with the PDF version to check CSS style changes).

Strangely enough, if you zoom into the PDF enough, the lines start to appear. After unsuccessfully poking around Salesforce&#8217;s <a title="StackExchange" href="http://salesforce.stackexchange.com" target="_blank">StackExchange</a> site, I started to think about the issue a bit and came up with the hypothesis that the PDF rendering must interpret the content of my cells being bigger than the HTML version. To test out that theory, I updated the CSS to include the following:

    td { padding: 1px; }

Fortunately, this appears to have solved the issue. I actually stepped through 0px to 1px in .1px increments to see the changes as different parts of the table started to show the borders again.

[<img class="aligncenter" alt="Screen Shot 2013-10-08 at 12.14.55 PM" src="http://michaelwelburn.com/wp-content/uploads/2013/10/Screen-Shot-2013-10-08-at-12.14.55-PM.png" width="502" height="130" />](http://michaelwelburn.com/wp-content/uploads/2013/10/Screen-Shot-2013-10-08-at-12.14.55-PM.png)