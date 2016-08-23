---
id: 290
title: Getting Started with Mixpanel for Website Analytics
date: 2013-03-12T07:00:22+00:00
author: Michael Welburn
guid: http://michaelwelburn.com/?p=290
permalink: /2013/03/12/getting-started-with-mixpanel-for-website-analytics/
categories:
  - Javascript
  - Programming
  - Weather Wardrobe
tags:
  - Analytics
  - KISSmetrics
  - mixpanel
---
Probably the best part about playing around with <a title="Introducing Weather Wardrobe: The quickest way to get dressed in the morning" href="http://michaelwelburn.com/2013/02/27/introducing-weather-wardrobe-the-quickest-way-to-get-dressed-in-the-morning/" target="_blank">building a web application</a> for fun is that I get a chance to try out a lot of different technologies and products. One of those technologies was something I talked about last week, <a title="Leveraging Redis with Ruby on Rails and Heroku" href="http://michaelwelburn.com/2013/03/04/leveraging-redis-with-ruby-on-rails-and-heroku/" target="_blank">Redis</a>. Today, I&#8217;m going to talk about an analytics product for web applications: <a title="Mixpanel" href="http://mixpanel.com" target="_blank">Mixpanel</a>.

<!--more-->

In the past I&#8217;ve used Google Analytics for tracking general site usage and metrics on side projects. However, this time I wanted to try out a service that let me track different user engagement events at a more granular level. There are two main players in the space, at least with regards to the different blogs that I read: Mixpanel & <a title="KISSmetrics" href="https://www.kissmetrics.com/" target="_blank">KISSmetrics</a>. While I didn&#8217;t do a comparison between the two, I referenced a blog <a title="KISSmetrics vs Mixpanel" href="http://sachagreif.com/analytics-showdown-kissmetrics-vs-mixpanel/" target="_blank">post</a> that I read a while ago and went with Mixpanel for the time being. Both of these products have a free tier, and if you reference the comments in the blog post I referenced you can get a bunch of extra free events to track.

The main value that each of these products bring is that I can set up analytics on any event using different <a title="Integration Libraries" href="https://mixpanel.com/docs/integration-libraries" target="_blank">languages</a> (I used Javascript). The simplest use case to derive value from would be if your web application had a sign up page. You know how many people signed up, because they are in your database, but you don&#8217;t know how many people loaded your sign up page, found it intimidating, and left. You could use Google Analytics to track the number of loads for that page and do some rough math, or you could have a unique analytics event track the number of people who loaded that page, and a second event track the number of people who actually submitted the form. This gives you a great view of what your conversion funnel is, and additionally lets you do some <a title="Ultimate Guide to A/B Testing" href="http://www.smashingmagazine.com/2010/06/24/the-ultimate-guide-to-a-b-testing/" target="_blank">A/B Testing</a> with different signup pages to check the conversion rate of each.

For <a title="Weather Wardrobe" href="http://weatherwardrobe.com" target="_blank">Weather Wardrobe</a>, I only have two events set up: one to track the number of times the page is loaded, and a second to track form submissions. Since I&#8217;m interested to see how many people who land on the page actually use it, this gives me some nice feedback.

In order to set this up, I needed to sign up at Mixpanel and create a new Project. Once the Project was created and I have the token, I grabbed the Javascript that I needed to include on the page from the <a title="JS Documentation" href="https://mixpanel.com/docs/integration-libraries/javascript" target="_blank">documentation</a>. I actually created two separate Projects in order to track analytics events in my local development environment separate from my production environment, which allows me to verify that the tracking code is working correctly locally without affecting the real data. Since I am using Ruby on Rails, I leveraged <a title="Setting Rails Environment Variables Outside of Version Control" href="http://michaelwelburn.com/2012/04/30/setting-rails-environment-variables-outside-of-version/" target="_blank">environment variables</a> to keep the code from having any hardcoded values.

    <!-- start Mixpanel --><script type="text/javascript">(function(e,b){if(!b.__SV){var a,f,i,g;window.mixpanel=b;a=e.createElement("script");a.type="text/javascript";a.async=!0;a.src=("https:"===e.location.protocol?"https:":"http:")+'//cdn.mxpnl.com/libs/mixpanel-2.2.min.js';f=e.getElementsByTagName("script")[0];f.parentNode.insertBefore(a,f);b._i=[];b.init=function(a,e,d){function f(b,h){var a=h.split(".");2==a.length&&(b=b[a[0]],h=a[1]);b[h]=function(){b.push([h].concat(Array.prototype.slice.call(arguments,0)))}}var c=b;"undefined"!==
    typeof d?c=b[d]=[]:d="mixpanel";c.people=c.people||[];c.toString=function(b){var a="mixpanel";"mixpanel"!==d&&(a+="."+d);b||(a+=" (stub)");return a};c.people.toString=function(){return c.toString(1)+".people (stub)"};i="disable track track_pageview track_links track_forms register register_once alias unregister identify name_tag set_config people.set people.increment people.append people.track_charge people.clear_charges people.delete_user".split(" ");for(g=0;g<i.length;g++)f(c,i[g]);b._i.push([a,
    e,d])};b.__SV=1.2}})(document,window.mixpanel||[]);
    <strong>mixpanel.init("<%= ENV['MIXPANEL'] %>");</strong></script><!-- end Mixpanel -->

Once that is included, you can start tracking your events. The first event that I was looking to track was the initial landing page being loaded. Because Weather Wardrobe is currently a single page application that, upon form submission, loads the same landing page with a URL parameter, I simply checked to see if that parameter existed, and if not tracked the event.

    var j$ = jQuery.noConflict();
    j$(document).ready(function()
    {
       if (<%= @zipcode.nil? %>)
       {
          mixpanel.track("Engage Landing Page");
       }
    }


<img class="alignnone size-full wp-image-332 aligncenter" alt="Screen Shot 2013-03-06 at 8.17.02 PM" src="http://michaelwelburn.com/wp-content/uploads/2013/03/Screen-Shot-2013-03-06-at-8.17.02-PM1.png" width="576" height="210" srcset="http://michaelwelburn.com/wp-content/uploads/2013/03/Screen-Shot-2013-03-06-at-8.17.02-PM1.png 576w, http://michaelwelburn.com/wp-content/uploads/2013/03/Screen-Shot-2013-03-06-at-8.17.02-PM1-300x109.png 300w" sizes="(max-width: 576px) 100vw, 576px" />

The second thing I was looking to track was form submissions. Normally I would just set up a jQuery <a title="jQuery Submit Event Handler" href="http://api.jquery.com/submit/" target="_blank">event handler on submit</a>, but Mixpanel has a particular method that automatically <a title="Track Forms" href="https://mixpanel.com/docs/integration-libraries/javascript-full-api#track_forms" target="_blank">tracks form submissions</a> that I leveraged instead. This also let me pass in parameters, which allows me to keep track of which postal codes are being searched on.

    var j$zipcode = j$('input#zipcode');
    mixpanel.track_forms("#zip-form", "Postal Code Form Submission", function(form)
    {
       return { postal_code : j$zipcode.val() };
    });


<img class="size-full wp-image-333 aligncenter" alt="Screen Shot 2013-03-06 at 8.17.25 PM" src="http://michaelwelburn.com/wp-content/uploads/2013/03/Screen-Shot-2013-03-06-at-8.17.25-PM1.png" width="576" height="209" srcset="http://michaelwelburn.com/wp-content/uploads/2013/03/Screen-Shot-2013-03-06-at-8.17.25-PM1.png 576w, http://michaelwelburn.com/wp-content/uploads/2013/03/Screen-Shot-2013-03-06-at-8.17.25-PM1-300x108.png 300w" sizes="(max-width: 576px) 100vw, 576px" />

As with most things I&#8217;m playing around with currently, I&#8217;m only scratching the surface on what I can do with analytics. While I am tracking trends, you can actually <a title="Mixpanel People" href="http://techcrunch.com/2012/07/02/mixpanel-people/" target="_blank">track individual users</a> by setting an identifier in an event and see what that user ends up doing on your site. Playing around with these tools really lets your mind think about how much data you can track and analyze in order to improve the page flow of your application and note where drop offs and lack of interaction is happening so you aren&#8217;t flying blind as a developer.
