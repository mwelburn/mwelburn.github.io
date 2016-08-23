---
id: 65
title: Confirming a User Really Wants to Leave a Page with JavaScript
date: 2013-04-08T07:00:40+00:00
author: Michael Welburn
guid: http://michaelwelburn.com/?p=65
permalink: /2013/04/08/confirming-a-user-really-wants-to-leave-a-visualforce-page/
categories:
  - Javascript
  - Programming
  - Salesforce
tags:
  - beforeunload
  - JavaScript
  - jQuery
  - Salesforce
  - Visualforce
---
Recently I needed to implement a solution that would stop users from leaving a pretty complex Visualforce form accidentally. The problem was that users would be mostly through a form and whether they misclicked a link on the page, tried to close the window, or hit the cancel button, didn&#8217;t always realize that they were about to lose all the changes that they made.

<img class="size-full wp-image-373 aligncenter" alt="Screen Shot 2013-04-07 at 10.53.37 AM" src="http://michaelwelburn.com/wp-content/uploads/2013/04/Screen-Shot-2013-04-07-at-10.53.37-AM.png" width="425" height="171" srcset="http://michaelwelburn.com/wp-content/uploads/2013/04/Screen-Shot-2013-04-07-at-10.53.37-AM.png 425w, http://michaelwelburn.com/wp-content/uploads/2013/04/Screen-Shot-2013-04-07-at-10.53.37-AM-300x120.png 300w" sizes="(max-width: 425px) 100vw, 425px" />

<!--more-->

The easiest way to handle this is to bind an event handler to beforeunload. This creates a prompt popup that the user can choose OK or Cancel, where cancel prevents them from navigating to a new URL (or closing the window). Leveraging jQuery&#8217;s <a title="jQuery Unload Event Handler" href="http://api.jquery.com/unload/" target="_blank">unload event handler</a> handles this pretty well, but I needed to go one step beyond that to do &#8220;selective user prompting&#8221;; I only wanted to prompt the user that they were about to lose their changes if they weren&#8217;t clicking the a couple of different buttons that were allowed (e.g. Save). The trick to that is to leverage a variable to prevent particular links from showing that gets set in a click handler of those buttons.

    var j$ = jQuery.noConflict();
    var tempDisableBeforeUnload = false;

    j$(window).on('beforeunload', function()
    {
       if (!tempDisableBeforeUnload)
       {
          return 'Leaving this page will lose your changes. Are you sure?';
       }
       else
       {
          //reset
          tempDisableBeforeUnload = false;
          return;
       }
    });

For instance, if you did not want the pop up to occur when your user is saving, you could set it up similar to the following. In my case, I was leveraging this on a Visualforce page via Salesforce, but this works the same on a standard input tag of type button as well.

    <apex:commandButton value="Save" action="{!save}" onclick="tempDisableBeforeUnload = true;"/>

For the most part this solution works quite well. I did have an unexpected <a title="onebeforeunload event is fired on click" href="http://codecorner.galanter.net/2011/10/20/onbeforeunload-event-is-fired-on-click/" target="_blank">hiccup</a> with newer versions of Internet Explorer (9+), where it appears that clicking on any anchor tag that has an href set fires the event. As a best practice, if you are firing Javascript it probably isn&#8217;t the best idea to wire it up to that attribute anyway, but unfortunately Salesforce leverages the href attribute quite a bit to fire javascript to open new windows for Lookups. This leads to the user getting confused, thinking that what they just click is making them leave their page when in reality is isn&#8217;t.

The resolution that was identifying these Lookups and setting additional click handlers (or in this case, setting a Visualforce onclick attribute) to set the variable to true on them as well. As an example, here is what setting the WhatId on a Task looks like in this case.

    <apex:inputField value="{!newTask.WhatId}" onclick="tempDisableBeforeUnload = true;"/>
