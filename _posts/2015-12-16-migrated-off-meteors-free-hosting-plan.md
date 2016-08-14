---
id: 1071
title: 'How I Migrated Off Meteor&#8217;s Free Hosting Plan'
date: 2015-12-16T07:22:42+00:00
author: Michael Welburn
layout: post
guid: http://michaelwelburn.com/?p=1071
permalink: /2015/12/16/migrated-off-meteors-free-hosting-plan/
twitterCardType:
  - summary
categories:
  - Javascript
  - Programming
  - Side Projects
tags:
  - Codeship
  - Compose
  - Meteor
  - Modulus
  - React
---
Over Thanksgiving I put some time into prototyping a small web application that I had been wanting to make for a few years, choosing the technology stack of <a href="https://www.meteor.com/" target="_blank">Meteor</a> & <a href="https://facebook.github.io/react/" target="_blank">React</a> as a way to become more familiar with both. There is a simple todo <a href="https://www.meteor.com/tutorials/react/creating-an-app" target="_blank">tutorial</a> using that stack that served as my baseline, and after completing that a couple days of development got me to a point I could test with some family members to check the site&#8217;s usefulness.

Fast forward a few weeks; the <a href="https://www.meteor.com/tutorials/blaze/deploying-your-app" target="_blank">free hosting</a> that Meteor provides (and I had been using) has been running into some <a href="https://forums.meteor.com/t/anybody-else-seeing-mongo-failures-on-meteor-com-hosted-apps/14495/21" target="_blank">hiccups with availability</a>, spurring me into looking to migrate the site onto something more reliable for the time being. Without any real background in Meteor, <a href="https://www.mongodb.org/" target="_blank">MongoDB</a>, or web hosting outside of <a href="https://heroku.com" target="_blank">Heroku</a>, I spent some time trying to identify how to quickly accomplish this task via my preferred way: searching the internet for blog posts.

I found some helpful tutorials which I&#8217;ll mention below in hopes of aggregating information into a post that can serve as more of an &#8220;end to end&#8221; guide that I couldn&#8217;t locate myself.

<!--more-->

_For full disclosure, I was also looking for a way to host the site roughly for free for the rest of the month, but I wanted a reasonable costing solution going forward if I wanted to continue to run the site. That being said, learning something new is always a motivator, so what I chose below isn&#8217;t necessarily my recommendation for someone looking for cheap personal SaaS services._

### Finding a New Hosting Provider

The first thing I ended up doing was taking a look at the hosting providers that support Meteor explicitly, which led me to <a href="https://modulus.io/" target="_blank">Modulus</a> as a step up from Meteor&#8217;s free hosting but without the high cost of Meteor&#8217;s <a href="https://www.meteor.com/why-meteor/pricing" target="_blank">Galaxy</a> hosting platform.

While doing some Googling, I came across this interesting Medium post called <a href="https://medium.com/@david_sykora/taking-meteor-apps-into-production-with-modulus-compose-and-codeship-54236d7f0cc#.44qa45xtb" target="_blank">Taking Meteor Apps Into Production with Modulus, Compose, and Codeship</a>. I had cross paths with <a href="https://codeship.com" target="_blank">Codeship</a> at an interview a few years ago and thought it was pretty cool, so I figured it would be fun to set up one of their free plans (however unnecessary that was for a 1 person prototype). I wasn&#8217;t particularly sold yet on paying for a separate MongoDB host in <a href="https://compose.io/" target="_blank">Compose</a>, but I figured I&#8217;d give it a shot for a free month as the administration tools are quite rich.

The Medium post mentioned above did a pretty good job of walking through setting up the services necessary to get the web application up and running, although some of the screenshots and information were outdated (e.g. there is no longer a free Compose plan that I could find). However, between that information and another Modulus tutorial on <a href="http://meteortips.com/deployment-tutorial/modulus/" target="_blank">meteortips.com</a> I was able to stand the site up.

I&#8217;d be remiss to not include the official Modulus documentation for setting up a project on their site, which is available <a href="http://help.modulus.io/customer/portal/articles/1647770-using-meteor-with-modulus" target="_blank">here</a>.

### Migrating the Existing Database

This part seemed like it would be relatively easy; MongoDB has some commands that allow export and import. I came across this blog post titled <a href="http://blog.daveroma.com/export-meteor-database-to/" target="_blank">Export a MongoDB Database Hosted on Meteor.com to a Different Host</a>, which seemed exactly what I wanted to do, and was a little more helpful to me as a novice than the Compose <a href="https://docs.compose.io/backups/mongodump-mongorestore.html" target="_blank">documentation</a>.

At first I tried to just use the Modulus MongoDB offering for my database, however I experienced issues (probably self-inflicted) trying to restore the data dump to it, receiving the vague &#8220;auth failed&#8221; response even though my credentials seemed correct. At that point, I decided to actually try out Compose &#8211; which worked right away.

I did run into the error referenced in that blog post and had to individually restore collections, however I had an extremely basic model with 2 collections so it wasn&#8217;t a big deal.

### Updating the Domain DNS records

While I was alright with giving out the Meteor free host generated _*.meteor.com_ URL to family as it was pretty easy to remember, Modulus gave me a much more&#8230;_unique_ URL for my project. They also allow you to set up a custom _*.mod.bz_ domain in the Administration settings, but that seemed less memorable for my wife, so I went ahead and pointed a domain registered with <a href="https://www.namecheap.com/" target="_blank">Namecheap</a> at it.

This <a href="http://help.modulus.io/customer/portal/articles/1700863-custom-domains" target="_blank">documentation</a> was pretty useful on setting that up (albeit a bit overwhelming for a personal project). The gist of it is that you need to update your Custom Domain on Modulus&#8217; Administration setting for your project with _*.yoursitename.com_ (assuming you are interested in mapping all subdomains and the root to your custom URL).

I went with the lazy (and repeatedly discouraged in the documentation) route of using CNAME instead of A records for my root domain because my attempts to use the A records weren&#8217;t taking effect as quickly as I would have liked (either that, or I configured incorrectly). Noting that the documentation&#8217;s screenshots are a bit outdated with regards to how Namecheap is set up today, I ended up having to set up a CNAME with a 301 Redirect in the actual Advanced DNS Panel.

### Summary

I haven&#8217;t written up anything about Meteor yet, but the concept behind it is very interesting to me as the data between client and server is meant to be kept in sync without having to write (too much) code to push live updates to the browser. As I primarily work on the Salesforce platform the potential for crossover seems rather limited today, but for fun projects the ability to spin up something with so much infrastructure baked in makes creating small &#8220;hack&#8221; projects a delight.

My use of React was much more limited; conceptually I followed the framework&#8217;s paradigms but I barely scratched the surface of its usefulness.

_**Note:** The prototype is a social group sharing site that isn&#8217;t built for more than 1 group, hence the lack of link or screenshots. _