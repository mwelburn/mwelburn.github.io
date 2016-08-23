---
id: 230
title: 'Introducing Weather Wardrobe: The quickest way to get dressed in the morning'
date: 2013-02-27T07:00:18+00:00
author: Michael Welburn
guid: http://michaelwelburn.com/?p=230
permalink: /2013/02/27/introducing-weather-wardrobe-the-quickest-way-to-get-dressed-in-the-morning/
categories:
  - API
  - Javascript
  - Programming
  - Ruby on Rails
  - Weather Wardrobe
tags:
  - HAMweather
  - heroku
  - httparty
  - jQuery
  - Ruby on Rails
  - weather
  - weather wardrobe
---
Many years ago (2008 to be precise), Joyce had the idea of a website that told her what to wear based on the weather. Over the years I&#8217;ve made a couple short lived attempts at implementation, but I always got hung up on one thing or another, trying to implement the fully mature solution for day 1. This often led to neglect after a few days, or wasting time debating possible features. I even went on an exhaustive search for the best weather API back in 2011 that I <a title="Comparing Weather APIs" href="http://michaelwelburn.com/2011/11/02/comparing-weather-apis/" target="_blank">blogged</a> about due to lack of information on the internet. Unfortunately, all that effort never yielded any tangible results beyond some lost time on my part.

However, recently I spent some time at <a title="LaunchPadLab" href="http://launchpadlab.com/" target="_blank">LaunchPadLab</a> hacking on a friend&#8217;s side project, <a title="The Brewer's Barrel" href="http://thebrewersbarrel.com/" target="_blank">The Brewer&#8217;s Barrel</a>. It got my juices flowing a bit to do some hacking of my own, and I remembered the once simple idea that I made overly complex (as I often do). I decided that I just wanted to ship a usable website. No fancy pictures of clothes. No user signups. A simple algorithm. All that could be upgraded later. And so I put together <a title="Weather Wardrobe" href="http://www.weatherwardrobe.com" target="_blank">Weather Wardrobe</a> last Saturday.

<img class="size-full wp-image-242 aligncenter" alt="Screen Shot 2013-02-26 at 8.16.39 PM" src="http://michaelwelburn.com/wp-content/uploads/2013/02/Screen-Shot-2013-02-26-at-8.16.39-PM.png" width="420" height="296" srcset="http://michaelwelburn.com/wp-content/uploads/2013/02/Screen-Shot-2013-02-26-at-8.16.39-PM.png 420w, http://michaelwelburn.com/wp-content/uploads/2013/02/Screen-Shot-2013-02-26-at-8.16.39-PM-300x211.png 300w" sizes="(max-width: 420px) 100vw, 420px" />

<!--more-->

### Getting Started

While I still have plenty of ideas (and a roadmap of features to add at my own leisure), I reflected a bit on what the core features needed to be for day 1 to simplify my effort. It really broke down into the following two features:

  * <span style="line-height: 15px;">Need to retrieve the weather based on the users location</span>
  * Need to take that weather information and give the user correct information

### The Back End

The first feature involved selecting a weather API from which I could retrieve the pertinent data, so I went back to my previous blog to see what the competitors were. Since my experimentation a few years ago, another site started to gain some popularity: <a title="HAMweather" href="http://www.hamweather.com" target="_blank">HAMweather</a>. It seemed very similar to my previous favorite, <a title="wunderground" href="http://wunderground.com" target="_blank">wunderground</a>, but provided some decent documentation (wunderground&#8217;s documentation is pretty barren) and took in a few different entry parameters I thought important (postal codes and geolocation). It also had a slightly more favorable developer (free) package than wunderground, so I decided to try it out.

After figuring out a couple of oddities with the API (namely that their version of what the temperature &#8220;feels like&#8221; is valid only at 6am, yet the rest of the forecast is for 6am to 6pm), I was ready to start building. I hadn&#8217;t built anything with Ruby on Rails in a while, so I settled on that as a way to quickly get something up and running (but also realizing I could quickly toss in some gems for future phases to handle things like user signups). There didn&#8217;t appear to be a current gem for HAMweather, so I ended up just using <a title="HTTParty" href="https://github.com/jnunemaker/httparty" target="_blank">HTTParty</a> to get the information, did a little parsing of my own, and set up a simple algorithm that returned different clothing values.

Once that piece was done, it was time for the front end. Originally I had planned to leverage HTML5 Geolocation or client side IP Address resolving to show the user the weather immediately. However, this initial site wasn&#8217;t using any AJAX due to limiting scope creep, and I didn&#8217;t want to load the page once, get that data, and force refresh it (seemed confusing to the user) so I decided to just add a postal code box for manual entry. Presumably Geolocation will show up pretty soon as it seems like an easy win for mobile users or a future hybrid application on the App Store.

### The Front End

My biggest fear when I build these side projects is the user interface, or more specifically, my inexperience making anything look good. This is the reason a lot of my projects seem to get 80% of the way and die off &#8212; the backend is built, but I don&#8217;t know what to do on the front end. Keeping with the simple theme, I stuck with the standard fonts, input boxes, and buttons from <a title="Twitter Bootstrap" href="http://twitter.github.com/bootstrap/" target="_blank">Bootstrap</a>. I also picked a background color that I associated with weather and didn&#8217;t hurt my eyes, and called it a day.

Once I got those two main features implemented, I added a couple other quick wins. The first was ensuring that the site was responsive and looked good on mobile devices, as I suspected that most people would probably check it out on their phones. This was pretty simple using Bootstrap and didn&#8217;t really affect my development effort. I played around with CSS Media Queries as well to fiddle with font sizes on the phone to ensure all the pertinent information showed up on the screen without having to scroll.

Another quick effort was doing some validation with javascript on the input box to try and filter out any non USA or Canada postal codes. Unfortunately, HAMweather doesn&#8217;t yet support querying for postal codes outside of those regions, so until I implement Geolocation (which works most everywhere) I am attempting to limit the values entered to let the overseas users know immediately, as well as to catch accidental errors.

### Extra Features

I added a couple of other features to Weather Wardrobe once it was functionally complete, partially out of curiosity, and partially out of need. Keep you eyes open for some follow up posts that dive into the use cases for each, but I integrated <a title="mixpanel" href="http://mixpanel.com" target="_blank">mixpanel</a> and Google for analytics as well as <a title="redis" href="http://redis.io/" target="_blank">redis</a> for caching. These were all pretty trivial things to add in, so I felt it was worth leveraging them from day 1.

I also included social media buttons for Twitter and Facebook at the last moment. I did not realize how slow they were to load compared to the rest of the site, but until I can remedy that I felt the tradeoff for ease of sharing was worth it (and with 40 Likes in the first 24 hours, I ended up being right).

Once these features were ready to go, I spun up a <a title="Heroku" href="http://heroku.com" target="_blank">Heroku</a> instance (keeping with the theme of free) and set up my custom domain (which I&#8217;ve owned for about 4 years), and did a little bit of informal testing before exposing it to the world.

[<img class="alignnone size-full wp-image-243 aligncenter" alt="Screen Shot 2013-02-26 at 8.33.31 PM" src="http://michaelwelburn.com/wp-content/uploads/2013/02/Screen-Shot-2013-02-26-at-8.33.31-PM.png" width="603" height="513" srcset="http://michaelwelburn.com/wp-content/uploads/2013/02/Screen-Shot-2013-02-26-at-8.33.31-PM.png 603w, http://michaelwelburn.com/wp-content/uploads/2013/02/Screen-Shot-2013-02-26-at-8.33.31-PM-300x255.png 300w" sizes="(max-width: 603px) 100vw, 603px" />](http://www.weatherwardrobe.com)

### Summary

I&#8217;m pretty pleased at what came out of a day&#8217;s work, and more excited that it actually works and people can use it. There are definitely a lot of small things to clean up, but functionality is the most important feature that I can deliver.

If you haven&#8217;t already, please check out <a title="Weather Wardrobe" href="http://www.weatherwardrobe.com" target="_blank">Weather Wardrobe</a>. I&#8217;d love to get any feedback about what you think about the site or any ideas you have that fit your needs! Get in touch with me in the comments below, the <a title="Contact" href="http://michaelwelburn.com/contact/" target="_blank">contact form</a>, or on Twitter at <a title="Twitter" href="http://twitter.com/MichaelWelburn" target="_blank">@MichaelWelburn</a>.
