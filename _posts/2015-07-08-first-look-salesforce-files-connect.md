---
id: 1042
title: 'First Look: Salesforce Files Connect'
date: 2015-07-08T21:17:50+00:00
author: Michael Welburn
layout: post
guid: http://michaelwelburn.com/?p=1042
permalink: /2015/07/08/first-look-salesforce-files-connect/
twitterCardType:
  - summary
cardImageWidth:
  - 280
cardImageHeight:
  - 150
categories:
  - Salesforce
tags:
  - Files Connect
  - Google Drive
  - OneDrive
  - sharepoint
---
I&#8217;ve been working with a few clients who have expressed concern about the quantity of files they want to migrate into Salesforce and whether <a href="http://www.salesforce.com/chatter/features/document-collaboration-software/" target="_blank">Salesforce Files</a> is the right place to move them, mostly from a cost perspective. Salesforce allows a certain amount of file storage in the organization, primarily driven off the <a href="https://help.salesforce.com/HTViewHelpDoc?id=limits_storage_allocation.htm" target="_blank">number and type of licenses</a> that the organization has, and then that limit can be increased by purchasing additional storage on top (not sure what that pricing actually is, but high enough that there are conversations about other options).

<!--more-->

<a href="https://www.salesforce.com/blog/2014/12/announcing-salesforce-files-connect-universal-file-sharing-speeds-up-business.html" target="_blank">Salesforce Files Connect</a> has shown up lately as one of the (no code) options for clients, particularly those who don&#8217;t want to support a custom integration. Files Connect is a way to access your Sharepoint, OneDrive, or Google Drive documents in a way that is visible within Salesforce (seen on the Files tab image below) in a read only manner, as well as allows you to attach those files to Chatter feed posts as well.

[<img class="aligncenter size-large wp-image-1046" src="http://michaelwelburn.com/wp-content/uploads/2015/07/Screen-Shot-2015-07-08-at-9.00.19-PM-1024x444.png" alt="Screen Shot 2015-07-08 at 9.00.19 PM" width="765" height="332" srcset="http://michaelwelburn.com/wp-content/uploads/2015/07/Screen-Shot-2015-07-08-at-9.00.19-PM-1024x444.png 1024w, http://michaelwelburn.com/wp-content/uploads/2015/07/Screen-Shot-2015-07-08-at-9.00.19-PM-300x130.png 300w" sizes="(max-width: 765px) 100vw, 765px" />](http://michaelwelburn.com/wp-content/uploads/2015/07/Screen-Shot-2015-07-08-at-9.00.19-PM.png)

I set up a proof of concept using Google Drive to get a better feel for the functionality using the <a href="https://help.salesforce.com/HTViewHelpDoc?id=admin_files_connect_overview.htm&language=en_US" target="_blank">setup guide</a> and found that I was up and running under half an hour. The basic overview is:

  1. Turn on Files Connect
  2. Create a permission set to access Files Connect, add it to your admin user
  3. Create an App under the <a href="https://console.developers.google.com/project" target="_blank">Google Developer Console</a>
  4. Create an Authentication Provider in Salesforce for Google (follow directions on what values to set in the setup guide for scoping)
  5. Update your Google App with the callback URL generated from the Authentication Provider
  6. Create an External Data Source for Google Drive, referencing the Authentication Provider

#### There are a couple of really neat use cases for Files Connect (and surely many more out there):

  1. Your company has a large amount of internal documentation that no one wants to migrate to Salesforce, but Salesforce is your system of use for employees and they need to get at it. This is your classic network shared drive or FTP scenario where you have GB upon GB (or TB) of data dumped onto a drive that is impossible to navigate through. You can use the **Named Principal** Identity Type to have a single authentication for a read only user for all your Salesforce users, preventing the need for a 1:1 Salesforce to External system user setup (and saving licenses in that external system). This is the same principal as setting up a &#8220;system account&#8221; on the backend that everyone authenticates through.
  2. Your company uses some external systems, but to post some content into Chatter (system of use) to have a discussion you have to upload a copy of that OneDrive file into Salesforce. This creates a second version&#8230;and soon a third&#8230;and fourth. Before you know it, no one knows what the official version is. In this case, you can set up the **Per User** Identity Type (1:1 Salesforce to External system user) where people can find the documents they want to share from these other systems and seamlessly link to them in Chatter so everything points to that one document in the external system.[<img class="aligncenter wp-image-1051 size-large" src="http://michaelwelburn.com/wp-content/uploads/2015/07/Screen-Shot-2015-07-08-at-9.21.27-PM-1024x488.png" alt="Screen Shot 2015-07-08 at 9.21.27 PM" width="765" height="365" srcset="http://michaelwelburn.com/wp-content/uploads/2015/07/Screen-Shot-2015-07-08-at-9.21.27-PM-1024x488.png 1024w, http://michaelwelburn.com/wp-content/uploads/2015/07/Screen-Shot-2015-07-08-at-9.21.27-PM-300x143.png 300w, http://michaelwelburn.com/wp-content/uploads/2015/07/Screen-Shot-2015-07-08-at-9.21.27-PM.png 1046w" sizes="(max-width: 765px) 100vw, 765px" />](http://michaelwelburn.com/wp-content/uploads/2015/07/Screen-Shot-2015-07-08-at-9.21.27-PM.png)
  3. You can turn on <a href="https://help.salesforce.com/HTViewHelpDoc?id=external_object_define.htm" target="_blank">external objects</a> for Files Connect &#8211; this one is pretty cool. If you haven&#8217;t seen external objects before, it basically instantiates a custom object in your organization that represents the basic metadata around the files you are exposing from your other systems. Then, because this metadata is on a Salesforce record, you can actually run a global search and find documents from these other systems&#8230;or you could query those files in some custom Apex/Visualforce implementation. Creates some interesting ideas for custom implementations&#8230;

[<img class="aligncenter wp-image-1045 size-large" src="http://michaelwelburn.com/wp-content/uploads/2015/07/Screen-Shot-2015-07-08-at-8.59.04-PM-1024x237.png" alt="Screen Shot 2015-07-08 at 8.59.04 PM" width="765" height="177" srcset="http://michaelwelburn.com/wp-content/uploads/2015/07/Screen-Shot-2015-07-08-at-8.59.04-PM-1024x237.png 1024w, http://michaelwelburn.com/wp-content/uploads/2015/07/Screen-Shot-2015-07-08-at-8.59.04-PM-300x69.png 300w" sizes="(max-width: 765px) 100vw, 765px" />](http://michaelwelburn.com/wp-content/uploads/2015/07/Screen-Shot-2015-07-08-at-8.59.04-PM.png)

I&#8217;m really interested to see where Files Connect goes in the next year. It is a file consumption interface today (and there are some limitations), but I could easily see some future enhancements to allow you to upload files directly into those other systems instead of Salesforce as the feature becomes more robust &#8211; maybe even an option to offload your file uploads to Chatter into one of these systems.

The best part is that you don&#8217;t have to worry about supporting a custom integration with these other systems &#8211; it&#8217;s all on Salesforce to keep it running.