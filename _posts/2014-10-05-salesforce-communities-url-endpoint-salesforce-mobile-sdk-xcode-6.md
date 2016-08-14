---
id: 1017
title: 'Salesforce Communities URL Endpoint with the Salesforce Mobile SDK &#038; Xcode 6'
date: 2014-10-05T21:11:18+00:00
author: Michael Welburn
layout: post
guid: http://michaelwelburn.com/?p=1017
permalink: /2014/10/05/salesforce-communities-url-endpoint-salesforce-mobile-sdk-xcode-6/
twitterCardType:
  - summary
cardImageWidth:
  - 280
cardImageHeight:
  - 150
categories:
  - iOS
  - Salesforce
---
Earlier this summer I was able to successfully (and quite easily) set up a **hybrid_remote** iOS application that pointed to a custom Salesforce community using the <a title="Salesforce Mobile SDK" href="https://developer.salesforce.com/page/Mobile_SDK" target="_blank">Salesforce Mobile SDK</a> version 2.2. The only real change required after running the installer was a <a title="Communities Login Endpoint" href="http://www.salesforce.com/docs/en/mobile_sdk/Content/communities_login_endpoint.htm" target="_blank">modification of the URL</a> referenced from **login.salesforce.com** to **custom-community.force.com**, which is similar to what you&#8217;d need to do if you were testing in a sandbox environment with **test.salesforce.com**. However, the most recent time I needed to do this, I noticed that version 2.3 of the SDK was released, and had been rewritten a bit.

<!--more-->

While the installation remained the same, some of the implementation details were a bit different. However, updating the **[project]-info.plist** file to set the **SFDCOAuthLoginHost** to my custom URL remained a consistent step. Unfortunately, I noticed that this wasn&#8217;t taking effect, and I was still seeing the login.salesforce.com domain when I ran the application.

I tried a variety of things, from deleting all the build files, deleting the derived data, deleting the app on the simulator, and finally recreating the project from scratch. None of these solutions worked, though to an extent they all have helped rectify issues I&#8217;ve had in Xcode or with the old SDK. I then tried to deploy to my actual phone, and lo and behold it worked! Finally posting to the GitHub repository&#8217;s issue tracker, I got a <a title="Salesforce Mobile SDK Issue" href="https://github.com/forcedotcom/SalesforceMobileSDK-iOS/issues/755" target="_blank">helpful result</a>.

It turns out, there is something in the latest version of Xcode (6) that is likely not picking up the defined token correctly in the .plist file. While I am only speculating based on what I know about the SDK, the temporary solution noted was to open the iOS Simulator and reset it under **<span style="color: #333333;">iOS Simulator > Reset Content and Settings</span>**<span style="color: #333333;">. Not the most ideal scenario, but something I can live with.</span>