---
id: 1029
title: 'Tips &#038; Tricks for Building Hybrid Remote Mobile Apps using Salesforce Mobile SDK &#038; Cordova'
date: 2015-05-04T21:40:32+00:00
author: Michael Welburn
guid: http://michaelwelburn.com/?p=1029
permalink: /2015/05/04/tips-tricks-for-hybrid-remote-mobile-apps-using-salesforce-mobile-sdk-cordova/
twitterCardType:
  - summary
cardImageWidth:
  - 280
cardImageHeight:
  - 150
categories:
  - Apex
  - iOS
  - Javascript
  - Programming
  - Salesforce
tags:
  - Android
  - Cordova
  - iOS
  - SalesforceMobileSDK
---
The <a href="https://developer.salesforce.com/page/Mobile_SDK" target="_blank">Salesforce Mobile SDK</a> is an extremely useful boilerplate for creating mobile apps on iOS and Android for Salesforce (<a href="https://developer.salesforce.com/blogs/engineering/2015/04/salesforce-mobile-sdk-windows-ready-microsoft-developers.html" target="_blank">and Windows now too</a>); in fact, it is so easy to setup that I&#8217;ve used it to prototype out some quick mobile apps for Salesforce Communities. The SDK supports 3 different flavors of mobile apps: fully <a href="https://developer.salesforce.com/page/Building_Native_iOS_Apps_With_the_Salesforce_Mobile_SDK_3.1" target="_blank">native</a>, <a href="https://developer.salesforce.com/page/Developing_Hybrid_Apps_with_the_Salesforce_Mobile_SDK" target="_blank">hybrid</a> local, and hybrid remote. Native and hybrid local require you to write an app outside of Visualforce &#8211; whether you choose to use Objective-C / Java for a native app or Javascript for a hybrid local app is up to you.

The third option involves using the SDK to create a mobile wrapper of your existing Salesforce app by pointing to your production or sandbox and providing a start URL to redirect to after login (see <a href="http://th3silverlining.com/2015/02/07/using-native-mobile-device-features-from-visualforce-with-phonegap/" target="_blank">here</a> for a quick start guide). Visualforce-based responsive apps are _perfect_ for the **hybrid remote** option that the SDK offers, which essentially wraps your Visualforce site in a wrapper that lets you distribute an app but make all of the future updates (outside the basic wrapper setup) via Visualforce. All of the Objective-C or Java &#8220;native&#8221; code has already been written by Salesforce, so unless you want to do something unique (like <a href="https://developer.salesforce.com/docs/atlas.en-us.pushImplGuide.meta/pushImplGuide/" target="_blank">push notifications</a>) you won&#8217;t write a single line of code.

<!--more-->

While the basic setup is very simple (there is actually no code changes, just a few modifications to XML or json files for configurations), I did run into quite a few brain teasers that, as a novice to Cordova, took me longer than I wish to admit to figure out. Below are some of the highlights in hopes that it saves someone else some time.

## Setting up the Mobile App to point to a Community

This isn&#8217;t so much of a holdup, as the <a href="https://developer.salesforce.com/docs/atlas.en-us.mobile_sdk.meta/mobile_sdk/communities_login_endpoint.htm" target="_blank">documentation</a> does tell you how to modify the URL that you authenticate against (production vs sandbox) for iOS and Android separately. You&#8217;ll need to instead point to the custom URL of your community if you want to go that route rather than _login.salesforce.com_ or _test.salesforce.com_. Note that your internal Salesforce users will not be able to authenticate to the mobile app (nor would they be able to authenticate against the community login page anyway).

## Displaying the Status Bar in iOS

Another well <a href="https://developer.salesforce.com/docs/atlas.en-us.mobile_sdk.meta/mobile_sdk/hybrid_status_bar_ios.htm" target="_blank">documented</a> item, but just a note that you&#8217;ll need to add a <a href="https://github.com/apache/cordova-plugin-statusbar" target="_blank">Cordova plugin</a> to actually show the status bar in iOS (otherwise your users won&#8217;t know what time it is). This isn&#8217;t necessary for Android.

## Debugging

As a bit of a novice with iOS (and being completely green with Android), debugging the emulator (and my actual device) was an interesting exercise. Considering that I was doing essentially all of my development in Visualforce, I needed a debug console you typically find in a browser. Fortunately both iOS and Android provide some solid tools based on their respective browsers. I leveraged this quick, informational <a href="http://developer.telerik.com/featured/a-concise-guide-to-remote-debugging-on-ios-android-and-windows-phone/" target="_blank">guide</a> for my own setup (while this references using actual devices, the setup steps for the browsers are applicable for emulators).

For iOS, assuming you&#8217;ve setup an Xcode project you can run the application in the standard iOS simulator or on your device. To debug your applications HTML / Javascript based pages, you can open Safari&#8217;s development tools to <a href="http://webdesign.tutsplus.com/articles/quick-tip-using-web-inspector-to-debug-mobile-safari--webdesign-8787" target="_blank">access the console</a> and then choose the current page.

[<img class="aligncenter size-medium wp-image-1030" src="http://michaelwelburn.com/wp-content/uploads/2015/05/Screen-Shot-2015-05-04-at-8.39.02-PM-300x110.png" alt="Safari Debug" width="300" height="110" srcset="http://michaelwelburn.com/wp-content/uploads/2015/05/Screen-Shot-2015-05-04-at-8.39.02-PM-300x110.png 300w, http://michaelwelburn.com/wp-content/uploads/2015/05/Screen-Shot-2015-05-04-at-8.39.02-PM.png 324w" sizes="(max-width: 300px) 100vw, 300px" />](http://michaelwelburn.com/wp-content/uploads/2015/05/Screen-Shot-2015-05-04-at-8.39.02-PM.png)

If you use the standard <a href="http://developer.android.com/tools/help/emulator.html" target="_blank">Android emulator</a> you can debug your hybrid application using <a href="https://developer.chrome.com/devtools/docs/remote-debugging" target="_blank">Chrome</a>. I found the Android emulator _extremely_ slow; it may be that there is some sort of fine tuning I can do with the settings, but I thought my settings were pretty generous and I have a pretty modern computer. For most of my actual testing I used <a href="https://www.genymotion.com/#!/" target="_blank">Genymotion</a>, though I didn&#8217;t get around to figuring out how to debug that emulator (I debugged the joint bugs on the iOS emulator, then verified they worked in Genymotion).

## Including cordova.js in Visualforce

As part of your implementation, you will need to include cordova.js in your Visualforce page as one of the includes. This file is the library that allows your Visualforce-based pages to access any native code that the SDK has (such as managing user sessions, authentication, logout, using the camera, etc.) via different <a href="http://plugins.cordova.io/#/" target="_blank">Cordova plugins</a>. The SDK automatically generates this file separately for iOS and Android, along with a couple standard plugins. The documentation notes that you can simply <a href="http://www.salesforce.com/docs/en/mobile_sdk/Content/hybrid_develop_hybrid_remote.htm" target="_blank">reference <em>//localhost/cordova.js</em></a> in your Visualforce <script> or <apex:includeScript> tags and it will be able to load your files without having to use static resources.

Unfortunately, while this seemed to work fine on my emulators, once I deployed to an actual device I did not have the same luck (even when I whitelisted * in the config.xml). I was able to debug my application to the point of realizing cordova.js was not loading. While it is a bit kludgy, I ended up taking the contents of the _www_ folder (separately for iOS and Android), zipping them up into a static resource, and serving them up directly from Salesforce.

While there might be a good workaround, I spent too much time trying to debug why there was a discrepancy (note that this was the 2.3 version of the SDK &#8211; as of now there is a <a href="https://developer.salesforce.com/blogs/engineering/2015/02/salesforce-mobile-sdk-3-1-unified-app-architecture-brings-unparalleled-flexibility.html" target="_blank">3.x version</a>).

## Intelligently loading the right cordova.js &#8211; at the right time

As mentioned above, there are separate cordova.js files (and folders) for iOS and Android. You&#8217;ll want to ensure you load the right one at the right time. You also don&#8217;t want to load cordova.js when you aren&#8217;t in a mobile app &#8211; it isn&#8217;t doing anything and is a wasted resource load. In fact, it created some oddities when it was loading for Internet Explorer, as IE on a tablet seemed to think it was an app that should be opened.

In order to know whether you are in a mobile app (rather than desktop or a responsive mobile browser), you can reference the User Agent. The easiest way to handle this is using Apex and check the headers of the current page&#8217;s <a href="https://www.salesforce.com/us/developer/docs/apexcode/Content/apex_system_pagereference.htm" target="_blank">PageReference</a> for **SalesforceMobileSDK**.

    public Boolean getIsMobileApp() {
      String userAgent = ApexPages.currentPage().getHeaders().get('User-Agent');
      return String.isNotBlank(userAgent) && userAgent.containsIgnoreCase('SalesforceMobileSDK');
    }

Once you have this setup, you can wrap your script include in an <apex:outputPanel> (ensure the layout is set to **none**) and set the rendered attribute to the result of that method.

    <apex:outputPanel layout="none" rendered="{!isMobileApp}">
      <apex:includeScript value="{!URLFOR($Resource.cordova, 'cordova.js')}"></apex:includeScript>
    </apex:outputPanel>

In order to determine whether you are on Android or iOS, you&#8217;ll use a very similar method that also checks the User Agent for iPhone / iPad and uses a pair of <apex:outputPanel> tags to differentiate what file to load. You can simply program one method to check for either iOS or Android (I found checking for iPhone / iPad easier) and then assume if you are in the mobile app but it is not iOS, it must be Android.

## External URLs

You&#8217;ll quickly find that if you have a URL to an external site, even if you set up your links to open in a new tab on your site, the mobile app will simply open them in the same web view. Since your mobile app has no back button, you&#8217;ll be stuck on that external link and can no longer access your mobile site (and will need to <a href="https://support.apple.com/en-us/HT201330" target="_blank">force the app to close</a> to reset it).

The easiest way to rectify that is the <a href="https://github.com/apache/cordova-plugin-inappbrowser" target="_blank">InAppBrowser</a> plugin. By using the <a href="https://developer.mozilla.org/en-US/docs/Web/API/Window/open" target="_blank">window.open()</a> javascript method, the InAppBrowser plugin will intercept your links and act upon them accordingly (note that it will not automatically evaluate any <a> tags with URL _href_ attributes).

For my purposes, I wanted to future proof the implementation a bit so I set up a Javascript click handler on all <a> tags rather than assuming future developers would _always_ use window.open() instead. This handler had a selector that checked for any URLs that had _http/https_ prefixes (my internal pages are always relative paths) or _target=&#8221;_blank&#8221; _to denote external URLs, though the use cases may vary amongst projects. In addition, I ensured that I only made modifications if the User Agent notified me that I was in a mobile app (similar to the section about loading cordova.js above)

## External URLs&#8230;and Android

While the InAppBrowser worked really well in iOS right from the start, I had a problem getting it to work on Android. It turns out, Android _requires** **_the **location** attribute on <a href="https://developer.mozilla.org/en-US/docs/Web/API/Window/open" target="_blank">window.open()</a> to be set to true. I had been setting it to false on iOS as it seemed unnecessary to give users the ability to type in a URL in a popup browser meant to show a single page. However, the X to close the popup browser on Android apparently is on the same toolbar as the URL, whereas in iOS they are in different sections. This one took me far too long to realize, as the <a href="http://cordova.apache.org/docs/en/3.2.0/cordova_inappbrowser_inappbrowser.md.html" target="_blank">documentation</a> doesn&#8217;t seem to explicitly state it is required (even though all the examples use it). I ended up happening upon a random message board post about it after a lengthy search, which I can&#8217;t even locate again to post a link to.

## Uploading your APK for Google Play

As of now there is a known issue with Google around the AndroidManifest.xml file that requires you to remove some tokens in your XML file and replace them with hardcoded values to get the upload to the Google Play developer site to work. See <a href="https://github.com/forcedotcom/SalesforceMobileSDK-Android/issues/430" target="_blank">here</a> for the GitHub ticket, and some comments around the best way to workaround for the time being (hopefully this gets fixed, so don&#8217;t forget to switch it back in the future).

To summarize, you&#8217;ll need to replace references to **<category android:name=&#8221;@string/app_package&#8221; />** with **<category android:name=&#8221;com.salesforce.androidsdk&#8221; />**
