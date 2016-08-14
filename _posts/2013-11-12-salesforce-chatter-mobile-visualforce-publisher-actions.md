---
id: 635
title: Why Salesforce Chatter Mobile Visualforce Publisher Actions are Unlocking Mobile for Everyone
date: 2013-11-12T07:00:12+00:00
author: Michael Welburn
layout: post
guid: http://michaelwelburn.com/?p=635
permalink: /2013/11/12/salesforce-chatter-mobile-visualforce-publisher-actions/
categories:
  - Salesforce
tags:
  - Chatter
  - mobile
  - Publisher Actions
  - Salesforce
  - Visualforce
---
Salesforce recently released an update to their <a title="Chatter Mobile" href="http://www.salesforce.com/mobile/" target="_blank">Chatter Mobile</a> application that I think has some major ramifications for those looking for low cost custom mobile solutions, and will help push more customers to handheld devices.

<!--more-->

Previously, Chatter Mobile did not have quite as much record related functionality as Salesforce&#8217;s other mobile offering, Salesforce Touch, although Salesforce has slowly been rolling out updates to make it more than just a <a title="Chatter" href="http://www.salesforce.com/chatter/overview/" target="_blank">Chatter</a> application. Touch, as I alluded to in this <a title="Salesforce.com Touch Customization Possibilities…are Limited" href="http://michaelwelburn.com/2013/09/16/salesforce-com-touch-customization-notes/" target="_blank">previous blog post</a>, has very limited customization potential, only allowed Visualforce tabs. As I mentioned before, the inability to have context around Visualforce pages takes away a lot of use cases that I have experienced in the past. However, the latest version of Chatter Mobile allows <a title="Publisher Actions" href="http://help.salesforce.com/apex/HTViewHelpDoc?id=actions_overview.htm" target="_blank">publisher actions</a> (among other things), which finally gives developers access to Visualforce pages with record context. This means you can now launch Visualforce that knows what record it wants to use as a starting point for updating, showing data about, or finding related records.

<a title="Mary Scotton" href="https://twitter.com/rockchick322004" target="_blank">Mary Scotton</a> provides a good overview of the capabilities of a Visualforce Publisher Action on her latest <a title="developerforce" href="http://blogs.developerforce.com/" target="_blank">developerforce</a> blog post, <a title="Chatter Mobile 4.2 and Visualforce Publisher Actions" href="http://blogs.developerforce.com/developer-relations/2013/11/chatter-mobile-4-2-and-visualforce-publisher-actions.html" target="_blank">Chatter Mobile 4.2 and Visualforce Publisher Actions</a>. In her demo, she shows off how Salesforce&#8217;s internal team can integrate a Visualforce Page with Twitter to tweet comments about upcoming Dreamforce sessions, pulling record data directly with Visualforce, all from within Chatter Mobile.

Prior to this update, Salesforce customers who were looking to extend the functionality of Salesforce&#8217;s mobile offerings were left with two options:

  * Build a Visualforce based mobile webapp, accessible via the browser.
  * Build a custom mobile application (hybrid or native).

Neither of these options is foolproof, as the former loses out on offline support and device tools like a camera, and the latter can be cost prohibitive and tied to particular platforms. In fact, both of these options require thought about getting the user experience of a full application right, because that is what you are building &#8212; a complete application.

What publisher actions in Chatter Mobile allow is developers to pick and choose places where their customers simply are looking to augment what is already available. In Mary&#8217;s example, Chatter Mobile handles 90% of what she wants to do. It is that extra 10%, the ability to quickly tweet about the record, that needs to be implemented. That equates to a large cost savings (a single Visualforce page) as opposed to a completely custom application.

Whereas customers in the past might have said &#8220;its not worth developing a mobile app for such a small use case&#8221;, they now can say &#8220;its only going to take a day or two to expose this functionality&#8221;. The cost-benefit analysis of developing these extensions to the mobile platform becomes a no brainer!

There is an unlimited of potential use cases for Visualforce publisher actions, and off the top of my head I can throw out the following:

  * Manage related lists (add / edit / delete) from a single page (similar to <a title="How to Create a Reusable Editable Table in Visualforce" href="http://michaelwelburn.com/2013/10/21/how-to-create-a-reusable-editable-table-in-visualforce/" target="_blank">this desktop-based solution</a>).
  * Ability to pull up a &#8220;checklist&#8221; of things to verify when talking to a prospect, and able to swipe off the ones you brought up right after a call instead of writing up notes in a text box.
  * Launch a page that pulls back 3rd party metrics or API driven information about the record

In addition to these new use cases, every AppExchange offering that provides custom Visualforce functionality around records now has to rush to offer a mobile version of their pages because as Chatter Mobile adoption grows, those users are going to start demanding their Salesforce apps be available within their phone.

I put together this live proof of concept of a checklist using out of the box jQuery Mobile components, and it fits into the Chatter Mobile theme pretty well. While not fully functional, implementing something like this is estimated in hours, not days or weeks.

<div style="text-align: center; width: 100%;">
  <a href="http://michaelwelburn.com/wp-content/uploads/2013/11/photo-3.png"><img class="aligncenter" style="display: inline; margin: 0 20px;" alt="photo 2" src="http://michaelwelburn.com/wp-content/uploads/2013/11/photo-2-169x300.png" width="169" height="300" /><img class="aligncenter size-medium wp-image-642" style="display: inline; margin: 0 20px;" alt="photo 3" src="http://michaelwelburn.com/wp-content/uploads/2013/11/photo-3-169x300.png" width="169" height="300" srcset="http://michaelwelburn.com/wp-content/uploads/2013/11/photo-3-169x300.png 169w, http://michaelwelburn.com/wp-content/uploads/2013/11/photo-3-576x1024.png 576w, http://michaelwelburn.com/wp-content/uploads/2013/11/photo-3.png 640w" sizes="(max-width: 169px) 100vw, 169px" /></a>
</div>

That&#8217;s not to say that fully custom applications don&#8217;t have a place in the ecosystem, particularly for highly customized use cases. But not all customers have the funding or need to build a fully custom application, and Visualforce publisher actions gets them one step closer to ensuring their complete business process is available from a handheld device.

&nbsp;