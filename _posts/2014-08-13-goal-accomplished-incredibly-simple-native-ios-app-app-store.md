---
id: 962
title: 'Goal Accomplished: A Simple Native iOS App on the App Store called PaceMyRace'
date: 2014-08-13T07:10:39+00:00
author: Michael Welburn
guid: http://michaelwelburn.com/?p=962
permalink: /2014/08/13/goal-accomplished-incredibly-simple-native-ios-app-app-store/
twitterCardType:
  - summary_large_image
cardImgSize:
  - small
cardImageWidth:
  - 280
cardImageHeight:
  - 150
cardImage_id:
  - 978
cardImage:
  - http://michaelwelburn.com/wp-content/uploads/2014/08/iTunesArtwork1.png
categories:
  - iOS
  - Programming
  - Side Projects
tags:
  - App Store
  - Pace Calculator
  - PaceMyRace
---
It&#8217;s been a long time coming, but I can finally check off **Publish an iOS App** off my list of <a title="New Year’s Resolutions for 2014" href="http://michaelwelburn.com/2014/01/01/new-years-resolutions-for-2014/" target="_blank">goals for the year</a> with the release of <a title="PaceMyRace" href="http://pacemyraceapp.com" target="_blank">PaceMyRace</a>. As mentioned recently in <a title="Progress Report: Catching Up On My 2014 Goals" href="http://michaelwelburn.com/2014/07/24/catching-2014-goals/" target="_blank">my midyear review</a>, I decided to take it upon myself to forget the excuse of waiting for Swift and iOS 8 to ship and just build a stupid simple Objective C application. I just needed something I could build in a weekend, and luckily I came across something I needed that did not require persisting any data or any external data sources. Perfect!

<!--more-->

## Background

Back in mid July, we ran another <a title="NY Giants 5K" href="http://www.nyrr.org/races-and-events/2014/new-york-giants-run-of-champions-5k" target="_blank">random race</a> (we&#8217;re still trying to qualify for <a title="9+1 NYC Marathon" href="http://www.nyrr.org/join-and-give/become-a-member/run-9-give-1" target="_blank">9+1 NYC Marathon guaranteed entry</a> for 2015). One of my pet peeves when I use apps like <a title="RunKeeper" href="http://runkeeper.com/" target="_blank">RunKeeper</a> (which is an awesome app) is that when I reach the end of the race, I know my time but my distance is always wrong. It&#8217;s not a matter of whether there is additional distance tracked on my run, but how much extra. On this particular race, it was only an extra .03 miles on a 3.1 mile race; however, my goal for this race was to get under a 9 minute pace. I ended up with a time of 27:53, which gave me a deceptively wrong pace of 8:55 minutes per mile (against a 3.13 mile distance). In an effort to figure out if I hit my goal, I had to open up mobile Safari, do a Google search, find a <a title="Pace Calculator" href="http://www.coolrunning.com/engine/4/4_1/96.shtml" target="_blank">rather clunky website</a>, and check my time against the actual race distance. I&#8217;m glad to say I barely made it: **8:59** minutes per mile.

Unfortunately, that was one of the more accurate distances that I&#8217;ve tracked during a race. Last weekend at the <a title="Big10 5K" href="http://btnbig10k.com/" target="_blank">Big10 5K</a>, my RunKeeper data tracked 4.64 miles instead of 3.1 (there was a long tunnel involved and the GPS lost connection). RunKeeper then congratulated me with a badge for running my fastest time ever at a 6:12 pace. Unfortunately for my ego, that was nowhere near true. Once again I had to rely on the same website to figure out my actual pace.

<div id="attachment_968" style="width: 610px" class="wp-caption aligncenter">
  <a href="http://michaelwelburn.com/wp-content/uploads/2014/08/Screen-Shot-2014-08-03-at-1.34.02-AM.png"><img class="wp-image-968 size-large" src="http://michaelwelburn.com/wp-content/uploads/2014/08/Screen-Shot-2014-08-03-at-1.34.02-AM-1024x708.png" alt="Interesting RunKeeper route" width="600" height="414" srcset="http://michaelwelburn.com/wp-content/uploads/2014/08/Screen-Shot-2014-08-03-at-1.34.02-AM-1024x708.png 1024w, http://michaelwelburn.com/wp-content/uploads/2014/08/Screen-Shot-2014-08-03-at-1.34.02-AM-300x207.png 300w, http://michaelwelburn.com/wp-content/uploads/2014/08/Screen-Shot-2014-08-03-at-1.34.02-AM-668x462.png 668w, http://michaelwelburn.com/wp-content/uploads/2014/08/Screen-Shot-2014-08-03-at-1.34.02-AM.png 1056w" sizes="(max-width: 600px) 100vw, 600px" /></a>

  <p class="wp-caption-text">
    Just running through the highway into McCormick Place. Nothing to see here.
  </p>
</div>

In the past I&#8217;ve typically just tried to do the math in my head and was happy with an approximation, but at this point I realized that I&#8217;d rather just have something that I could quickly look up the actual values. I did a quick search on the App Store and found that the existing apps were kind of ugly (there isn&#8217;t really money to be made in this particular market). Combining my goal of making an app with the sheer simplicity of this personal need, it was the perfect excuse to spend a Saturday night cranking out the <a title="Lean Startup" href="http://theleanstartup.com/principles" target="_blank">MVP</a>.

## The Build

As I noted last month, it has been a long time since I&#8217;ve even looked at any Objective C code. I went through quite a few <a title="iOS Apprentice" href="http://www.raywenderlich.com/store/ios-apprentice" target="_blank">tutorials</a> and books earlier in the Spring, and while the syntax came back pretty quickly, I&#8217;m still extremely raw in my knowledge of the SDK. Fortunately this app was an exercise in dragging components around on <a title="Interface Builder" href="https://developer.apple.com/xcode/interface-builder/" target="_blank">Interface Builder</a> and writing some pretty simple controller logic. I pretty much just decided to not customize the interface at all in an effort to simply finish it in a night, though I spent a little time cleaning it up and adding an About page the next day.

## Post-Coding Tasks

Somehow I spent quite a bit more time on the post-code portion of this app. At a high level, I ended up having to do the following:

  * Come up with a name
  * Make an icon for the app
  * Figure out how to submit to the App Store
  * Build a website landing page

Per usual, coming up with a name that I liked was a frustrating effort. I came up with a bunch and got a consensus from my family on which they preferred, rather than agonizing too much on something that was meant to be a fun weekend project (with no monetary implications).

Making an icon for the App Store was another interesting endeavor. Since I have almost no knowledge of Photoshop, I searched around and found a cool website that lets you download a template for generating the icons called <a title="App Icon Template" href="http://appicontemplate.com/ios7" target="_blank">App Icon Template</a>. I was able to use this to create a single image and automatically generate all the smaller images that were needed. Unfortunately all the templates in the world can&#8217;t immediately make me better at coming up with a logo idea or bringing that logo to life. However, once again I found myself adhering to the &#8220;weekend project&#8221; mantra and scribbled together an idea while waiting to leave the apartment on Sunday. While it was rough around the edges, I found myself liking it. I then polished it up a bit (still using my freehand mouse drawing with the exception of the perfect circle I could never draw) and patted myself on the back.

<div id="attachment_977" style="width: 410px" class="wp-caption aligncenter">
  <a href="http://michaelwelburn.com/wp-content/uploads/2014/08/iTunesArtwork.png"><img class="size-full wp-image-977" src="http://michaelwelburn.com/wp-content/uploads/2014/08/iTunesArtwork.png" alt="PaceMyRace - Icon 1.0" width="400" height="400" srcset="http://michaelwelburn.com/wp-content/uploads/2014/08/iTunesArtwork.png 512w, http://michaelwelburn.com/wp-content/uploads/2014/08/iTunesArtwork-150x150.png 150w, http://michaelwelburn.com/wp-content/uploads/2014/08/iTunesArtwork-300x300.png 300w" sizes="(max-width: 400px) 100vw, 400px" /></a>

  <p class="wp-caption-text">
    This was the original sketch. It looks like a child drew it.
  </p>
</div>

<div id="attachment_978" style="width: 410px" class="wp-caption aligncenter">
  <a href="http://michaelwelburn.com/wp-content/uploads/2014/08/iTunesArtwork1.png"><img class="size-full wp-image-978" src="http://michaelwelburn.com/wp-content/uploads/2014/08/iTunesArtwork1.png" alt="PaceMyRace - Icon 2.0" width="400" height="400" srcset="http://michaelwelburn.com/wp-content/uploads/2014/08/iTunesArtwork1.png 512w, http://michaelwelburn.com/wp-content/uploads/2014/08/iTunesArtwork1-150x150.png 150w, http://michaelwelburn.com/wp-content/uploads/2014/08/iTunesArtwork1-300x300.png 300w" sizes="(max-width: 400px) 100vw, 400px" /></a>

  <p class="wp-caption-text">
    The final version. I think this is my design magnum opus.
  </p>
</div>

The App Store deployment was a bit of a hassle to figure out. Between the different profiles & certificates, iOS development already gives me a headache to try and manage all the necessary components of the developer lifecycle. Fortunately, they have submission process <a title="App Distribution Guide" href="https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/AppDistributionGuide/ConfiguringYourApp/ConfiguringYourApp.html" target="_blank">documented</a> pretty well so I was able to simply follow that, but I had to spend quite a bit of time trying to figure out why different parts of my developer account weren&#8217;t showing up right when I was going through different wizards. I&#8217;m pretty sure just this part (tying up the loose ends related to submission and going through all the steps) took almost as long as the coding.

Building the <a title="PaceMyRace" href="http://pacemyraceapp.com" target="_blank">website</a> was the least difficult part of all this, since all I really wanted was a landing page with no interaction. I simply took a couple pictures I already had, tried my best to follow Apple&#8217;s <a title="App Store Marketing Guidelines" href="https://developer.apple.com/app-store/marketing/guidelines/" target="_blank">App Store Marketing Guidelines</a>, and dumped the single page of <a title="Bootstrap" href="http://getbootstrap.com/" target="_blank">Bootstrap</a>-styled HTML onto a <a title="GitHub Page" href="https://pages.github.com/" target="_blank">GitHub Page</a>. I did get to learn a bit about how to set the right **meta** tags for <a title="Twitter Cards" href="https://dev.twitter.com/docs/cards" target="_blank">Twitter Cards</a> and <a title="Facebook" href="https://developers.facebook.com/docs/opengraph/howtos/maximizing-distribution-media-content" target="_blank">Facebook Sharing</a> via the <a title="Open Graph" href="http://ogp.me/" target="_blank">Open Graph protocol</a> (which really just means, when you share the website on either site it shows a picture, title, & summary).

[<img class="aligncenter size-full wp-image-984" src="http://michaelwelburn.com/wp-content/uploads/2014/08/Screen-Shot-2014-08-06-at-8.59.07-AM.png" alt="Facebook Share" width="472" height="159" srcset="http://michaelwelburn.com/wp-content/uploads/2014/08/Screen-Shot-2014-08-06-at-8.59.07-AM.png 472w, http://michaelwelburn.com/wp-content/uploads/2014/08/Screen-Shot-2014-08-06-at-8.59.07-AM-300x101.png 300w" sizes="(max-width: 472px) 100vw, 472px" />](http://michaelwelburn.com/wp-content/uploads/2014/08/Screen-Shot-2014-08-06-at-8.59.07-AM.png)

&nbsp;

<a title="Twitter Card Validator" href="https://dev.twitter.com/docs/cards/validation/validator" target="_blank">Twitter&#8217;s validator</a> rejected my meta tags until the app actually passed review and was on the store, and I had all sorts of issues with <a title="Facebook Validator" href="https://developers.facebook.com/tools/debug/" target="_blank">Facebook&#8217;s validator</a> disliking my **og:image** tag even though my image size was well over their minimum. I tried changing files types, changing where I was serving the image up from, etc. After doing a lot of Googling and coming across <a title="StackOverflow Post" href="http://stackoverflow.com/a/15898292/1038799" target="_blank">many others lamenting the same thing</a>, I just gave up when it gave me a warning but still picked up the image I wanted.

[<img class="aligncenter wp-image-988" src="http://michaelwelburn.com/wp-content/uploads/2014/08/Screen-Shot-2014-08-06-at-7.18.32-PM.png" alt="Facebook validator" width="700" height="92" srcset="http://michaelwelburn.com/wp-content/uploads/2014/08/Screen-Shot-2014-08-06-at-7.18.32-PM.png 938w, http://michaelwelburn.com/wp-content/uploads/2014/08/Screen-Shot-2014-08-06-at-7.18.32-PM-300x39.png 300w, http://michaelwelburn.com/wp-content/uploads/2014/08/Screen-Shot-2014-08-06-at-7.18.32-PM-668x87.png 668w" sizes="(max-width: 700px) 100vw, 700px" />](http://michaelwelburn.com/wp-content/uploads/2014/08/Screen-Shot-2014-08-06-at-7.18.32-PM.png)

## The Review Process

For whatever reason, part of my interest in building an iOS app was to see what the end to end app submission process with Apple was like. It was actually pretty boring. After submitting the app, I had to wait 8 days for the review to happen. Once approved, the app just shows up on the app store. No interaction necessary from me.

## Overall

Going through the process and finishing something was a nice boost for me, and has me ready to work on a couple other really basic ideas. There are a few tweaks I&#8217;d like to make to PaceMyRace, but since it has pretty basic functionality there isn&#8217;t too much necessary to add.
