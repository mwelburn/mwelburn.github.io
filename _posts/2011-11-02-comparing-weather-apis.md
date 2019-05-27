---
id: 19
title: Comparing Weather APIs
date: 2011-11-02T00:49:00+00:00
author: Michael Welburn
guid: http://michaelwelburn.com/2011/11/02/comparing-weather-apis/
permalink: /2011/11/02/comparing-weather-apis/
categories:
  - API
  - Programming
tags:
  - AccuWeather
  - API
  - comparison
  - Google Weather
  - HAMweather
  - National Weather Service
  - project
  - SimpleGeo
  - SimpleGeo Context
  - weather
  - Weather Bug
  - Weather Channel
  - Weather Underground
  - World Weather Online
  - Wunderground
  - yahoo weather
redirect_from:
  - /post/12220032768/comparing-weather-apis/
  - /post/12220032768/
---
<p class="p1">
  I’ve been searching the internet recently for a comparison between all the major weather APIs for a side project. Unfortunately, there isn’t much out there comparing the different APIs, and any links are from a few years ago. Therefore, I decided to take what I found during my research and post it online, hoping that someone else in a similar situation can make use of this in some way.
</p>

<!--more-->

My primary objective involved finding a service that provides the daily weather forecast. Within this, I was looking for high/low temperatures, wind speed, conditions (most websites seem to have a myriad of conditions mapped to integers) and if possible humidity. While originally I really only need the current day, to scale past an initial product I will need the ability to do some sort of forecasting into the future, so that is a definite secondary concern. Anything outside of these are gravy for future use. Additionally, since this is going to be a slow development effort, a free model that scales into a paid model (or a free model that stays free) are definitely appreciated. I’ll be glad to worry about scaling the number of API calls when that day comes.

<p class="p1">
  It seems like most weather services update once an hour at the most, so developers are encouraged to cache API results to limit the number of calls for the same location. For any service that has different tiers of plans based on number of API calls, that helps limit the number of outgoing requests (and keeps your application moving quicker anyway). However, making requests with the IP address directly kind of makes caching a moot point, unless you convert it to a zip code first, which take away a nice feature some of the APIs have in terms of being able to make a request using the IP address for the location.
</p>

<!-- more -->

* * *

<p class="p1">
  Without further delay, a rundown of the contenders:
</p>

<p class="p1">
  <a href="http://michaelwelburn.com/wp-content/uploads/2011/11/tumblr_lu0eh2KkOK1r1vsop.gif"><img class="alignnone size-full wp-image-47" alt="Yahoo!" src="http://michaelwelburn.com/wp-content/uploads/2011/11/tumblr_lu0eh2KkOK1r1vsop.gif" width="213" height="30" /></a>
</p>

<p class="p1">
  <span class="s1"><a href="http://developer.yahoo.com/weather/" target="_blank"><strong>Yahoo! Weather</strong></a></span>&#8211; The first service that comes to mind, Yahoo! Weather is free for non-commercial use. This service is the only one to make use of “Where on Earth ID” (or WOEID) instead of zip codes. Apparently WOEIDs are able to denote things like landmarks, cities, etc. Unfortunately that means Chicago has a single WOEID instead of a myriad of zip codes, so I would assume the accuracy would be a bit worse. Additionally, I’d have to write some code to grab the zip code from the user, call a Yahoo API to get the WOEID (and then store that lookup in a database for the future), and then finally use that WOEID for my API call to retrieve the weather. The response from Yahoo! Weather is basically an RSS feed.<strong> </strong>I was not overly thrilled with the WOEID (call me shallow), but the lack of forecasting beyond the current day strengthened my reasoning to skip out on Yahoo! Weather.
</p>

<p class="p1">
  <a href="http://michaelwelburn.com/wp-content/uploads/2011/11/tumblr_lu0egv3Y5E1r1vsop.jpg"><img class="alignnone size-full wp-image-54" alt="WeatherBug" src="http://michaelwelburn.com/wp-content/uploads/2011/11/tumblr_lu0egv3Y5E1r1vsop.jpg" width="257" height="50" /></a>
</p>

<p class="p1">
  <span class="s1"><a href="http://weather.weatherbug.com/desktop-weather/api.html" target="_blank"><strong>Weather Bug</strong></a></span>&#8211; Weather Bug’s website was a pain for me to use, which immediately was annoying to me. It was both slow and not really intuitive, so I wasn’t all that excited to evaluate them after that. They have an XML and pipe delimited (CSV?) format, and their documentation seemed really unhelpful and basically just hard to understand how to actually make calls. From what I can gather, you can use zip code or coordinates to query for the weather, and there seems to be the ability to get live weather and forecasts. Unfortunately I can’t decrypt the documentation well enough to really see what kind of information you can get out of the responses without really digging in and setting up some example calls, so that was enough to make me skip past it.
</p>

<p class="p1">
  <a href="http://michaelwelburn.com/wp-content/uploads/2011/11/tumblr_lu0egqRlC81r1vsop.jpg"><img class="alignnone size-medium wp-image-53" alt="wunderground" src="http://michaelwelburn.com/wp-content/uploads/2011/11/tumblr_lu0egqRlC81r1vsop-300x41.jpg" width="300" height="41" srcset="http://michaelwelburn.com/wp-content/uploads/2011/11/tumblr_lu0egqRlC81r1vsop-300x41.jpg 300w, http://michaelwelburn.com/wp-content/uploads/2011/11/tumblr_lu0egqRlC81r1vsop.jpg 436w" sizes="(max-width: 300px) 100vw, 300px" /></a>
</p>

<p class="p1">
  <span class="s1"><a href="http://www.wunderground.com/weather/api/" target="_blank"><strong>Weather Underground</strong></a></span> &#8211; Weather Underground is a service I’d never heard of before, but after doing some research apparently they are pretty popular. They have three different feature plans, depending on how much information you are looking for. For what I am after, the base plan (Stratus) would be sufficient. After choosing a feature plan, they have different usage plans depending on the number of API calls. Fortunately there is a developer plan for 500 API calls a day. Another nice feature is that you are allocated 3 “raindrops”, which are essentially helping out if you happen to be really popular on a certain day by not rate limiting you. Their responses can either be JSON or XML, and you can query location based on a myriad of information (zip code, coordinates, city, IP address, etc). The service provides the ability to forecast, and all of the pieces of data that I am looking to leverage (albeit only the temperature and conditions in the forecast, though adding a percent chance of precipitation is nice). This site also was the cleanest and simplest to look at as a developer, which is always a plus.
</p>

<p class="p1">
  <strong>Update (11/9/2011):</strong> After speaking with representation from Weather Underground, they mentioned that humidity and wind in weather forecasts were currently in development.
</p>

<p class="p1">
  <strong>Update (11/10/2011):</strong> I’ve been told that humidity and wind have been added to the weather forecasts. From what I researched, this definitely gives their forecasting API a step up over competition, which from what I recall mostly don’t support wind (and none support humidity).
</p>

<p class="p1">
  <a href="http://michaelwelburn.com/wp-content/uploads/2011/11/tumblr_lu0egkAFOU1r1vsop.png"><img class="alignnone size-full wp-image-52" alt="World Weather Online" src="http://michaelwelburn.com/wp-content/uploads/2011/11/tumblr_lu0egkAFOU1r1vsop.png" width="184" height="80" /></a>
</p>

<p class="p1">
  <span class="s1"><a href="http://www.worldweatheronline.com/weather-api.aspx" target="_blank"><strong>World Weather Online</strong></a></span> &#8211; World Weather Online is another service I was unfamiliar with. They also have a free service, with the request of not exceeding 500 API calls per hour. With caching of data based on zip code, and the site only updating the weather every 3-4 hours, that would be more than enough for now. The service is also free for commercial and non-commercial use, only requesting a link to be placed on your site (and many of these sites would enjoy/require the same anyway). They do have a premium option if you don’t want to display a reference to them (and you get some additional features). The responses are in XML, JSON, or CSV format, and the requests can be made with the city name, zip code, IP address, or coordinates. Additionally, its forecasts included some details that Weather Underground didn’t (wind speed, predicted precipitation in MM). Based purely on price and features, this service seemed to take the cake as long as you didn’t mind that your weather updates weren’t as frequent as some of the other sites that update hourly.
</p>

<p class="p1">
  <a href="http://michaelwelburn.com/wp-content/uploads/2011/11/tumblr_lu0efwiwk11r1vsop.png"><img class="alignnone size-full wp-image-51" alt="The Weather Channel" src="http://michaelwelburn.com/wp-content/uploads/2011/11/tumblr_lu0efwiwk11r1vsop.png" width="110" height="100" /></a>
</p>

<p class="p1">
  <span class="s1"><strong><a href="http://portal.theweatherchannel.com/" target="_blank">The Weather Channel</a></strong></span>&#8211; The Weather Channel apparently decided that it’s API would be a pay to use model. Developers can test out the calls via a console, however it doesn’t look like there is any free model to actually test within a custom application. This pretty much took this product out of the picture for me, so I didn’t spend any additional time researching it. This was too bad, because at a glance it seemed to have quite detailed forecasting, including humidity, something the other APIs lacked. The Weather Channel provides their responses in XML and JSON.
</p>

<p class="p1">
  <a href="http://michaelwelburn.com/wp-content/uploads/2011/11/tumblr_lu0efmtoEa1r1vsop.jpg"><img class="alignnone size-full wp-image-50" alt="AccuWeather" src="http://michaelwelburn.com/wp-content/uploads/2011/11/tumblr_lu0efmtoEa1r1vsop.jpg" width="278" height="58" /></a>
</p>

<p class="p1">
  <strong><span class="s1"><a href="http://www.accuweather.com/" target="_blank">AccuWeather</a></span></strong>&#8211; I saw some people posting that AccuWeather had an API, but I had no luck finding it on their site.
</p>

<p class="p1">
  <strong>Update (7/10/2013): </strong>Per a comment below, the API is available <a title="AccuWeather" href="http://api.accuweather.com/" target="_blank">here</a>.
</p>

<p class="p1">
  <a href="http://michaelwelburn.com/wp-content/uploads/2011/11/tumblr_lu0eewu5cx1r1vsop.png"><img class="alignnone size-full wp-image-49" alt="National Weather Service" src="http://michaelwelburn.com/wp-content/uploads/2011/11/tumblr_lu0eewu5cx1r1vsop.png" width="100" height="100" /></a>
</p>

<p class="p1">
  <span class="s1"><a href="http://graphical.weather.gov/xml/rest.php" target="_blank"><strong>National Weather Service</strong></a></span>&#8211; The National Weather Service actually has some pretty explicit ways to make requests to their service, though primarily based on zip code or coordinates. This was by far the most complex looking piece of documentation out of all these sites, so novice developers might be overwhelmed trying to figure out which of the sample queries would be most applicable. The data does update relatively frequently (every 45 minutes to an hour), and this is basically the data that a lot of other services use by prettying up the request and response for you. It appears that the response is in XML, although their XML is a bit less readable than the other services (seems to be more indexed based within a bunch of nodes, so it is on the developer to do that parsing) and from my brief experimentation it seems like it is just returning temperature both for the current day and the forecasted days. I very well may be missing something, but I would prefer the weather integration “just work”.
</p>

<p class="p1">
  <a href="http://michaelwelburn.com/wp-content/uploads/2011/11/tumblr_lu0eeoqWW51r1vsop.png"><img class="alignnone size-full wp-image-48" alt="Google" src="http://michaelwelburn.com/wp-content/uploads/2011/11/tumblr_lu0eeoqWW51r1vsop.png" width="116" height="41" /></a>
</p>

<p class="p1">
  <strong>Google Weather API &#8211;</strong> This feed is unofficial, and is what Google uses to power the weather portion of some of their applications. However, they could <a href="http://www.google.com/support/forum/p/apps-apis/thread?tid=718560d8c2c913ff&amp;hl=en" target="_blank"><span class="s1">change the schema</span></a> or shut it down at any time, so that’s a negative. The unofficial part means there is no official documentation, so prospective developers need to either leverage research that a previous developer did and <a href="http://blog.programmableweb.com/2010/02/08/googles-secret-weather-api/" target="_blank"><span class="s1">posted</span></a> to the internet, or needs to comb through the requests and responses manually to figure it out. It seems like multiple people refer to this as the best API, as the XML returned is very readable. I’m under the assumption that someone who had more free time to really figure out the power of Google Weather API would be pleased, but unfortunately I am not that person at the moment.
</p>

<p class="p1">
  <strong>Update:</strong> This option is no longer available, as Google has <a title="Google Kills Weather API" href="http://www.lifehacker.com.au/2012/08/google-silently-kills-popular-api-breaks-weather-apps-everywhere/" target="_blank">turned it off</a>.
</p>

<p class="p1">
  <a href="http://michaelwelburn.com/wp-content/uploads/2011/11/tumblr_lu0eegognT1r1vsop.png"><img class="alignnone size-full wp-image-56" alt="SimpleGeo" src="http://michaelwelburn.com/wp-content/uploads/2011/11/tumblr_lu0eegognT1r1vsop.png" width="142" height="27" /></a>
</p>

<p class="p1">
  <strong><span class="s1"><a href="https://simplegeo.com/docs/api-endpoints/simplegeo-context#weather" target="_blank">SimpleGeo Context</a></span></strong> &#8211; This service is much more generic with regards to what it shows for location based weather, as it merely tells you the conditions and no statistics. This isn’t what I am looking for (although it definitely offers a lot of other cool location based information). Additionally, there’s no free version (though there is a 60 day trial), and the entry level price is $9.99. This product really just doesn’t provide what I am looking for.
</p>

<p class="p1">
  <strong>Update:</strong> This option is no longer <a title="http://blog.programmableweb.com/2012/01/12/simplegeo-apis-closed-but-places-data-is-open/" href="http://blog.programmableweb.com/2012/01/12/simplegeo-apis-closed-but-places-data-is-open/" target="_blank">available</a>.
</p>

<p class="p1">
  <img class="alignnone size-full wp-image-100" alt="hw_logo_light_bg" src="http://michaelwelburn.com/wp-content/uploads/2011/11/hw_logo_light_bg.png" width="200" height="55" />
</p>

<p class="p1">
  <strong>Update (2/17/2013): </strong><a title="HAMweather" href="http://www.hamweather.com/support/documentation/" target="_blank">HAMweather</a> &#8211; HAMweather looks to be another very viable offering for developers. They have a developer account that allows 750 API calls a day (the first paid offering is 5,000 API calls at $25/month) and the response information for the endpoints looks pretty exhaustive in terms of information that I usually find necessary (temperature, humidity, feels like, precipitation). Another nice feature of HAMweather is that they have SDKs for Javascript, iOS, and Android to help jumpstart development.
</p>

<p class="p1">
  <strong>Update (5/26/2019):</strong> HAMweather is now <a title="AerisWeather" href="https://www.aerisweather.com/" target="_blank">AerisWeather</a>.
</p>

## Forecast.io

<p class="p1">
  <strong>Update (3/26/2013): </strong><a title="Forecast.io" href="https://developer.darkskyapp.com/" target="_blank">Forecast.io</a> &#8211; The company behind the <a title="Dark Sky App" href="http://darkskyapp.com/" target="_blank">Dark Sky</a> iOS app has <a title="Announcing Forecast.io" href="http://blog.forecast.io/post/46290267206/announcing-forecast" target="_blank">released</a> an API that aggregates data from a bunch of sources. Their pricing model is current 1,000 free API calls daily, with every subsequent 10,000 calls costing another dollar. Their endpoints are pretty basic at first glance, but the pricing model is very attractive compared to some of other options. As a side note, the forecast.io web and mobile <a title="Forecast.io" href="http://forecast.io" target="_blank">applications</a> are pretty impressive visually.
</p>

## OpenWeatherMap

<p class="p1">
  <strong>Update (4/2/2013): </strong><a title="OpenWeatherMap" href="http://openweathermap.org/" target="_blank">OpenWeatherMap</a> &#8211; This is a free service that provides forecast and weather data. The documentation is less easily navigated than some of the other (paid) sites, but it looks to return most of the relevant information that most users are probably interested in (temperature, humidity, wind, precipitation, etc. It looks like geolocation is the only parameter than you can use for location identifying at the moment.
</p>

* * *

<p class="p1">
  After looking at all these options <strong>(in late 2011)</strong>, for both ease of use, features, and price, I ended up between World Weather Online and Weather Underground. While my development is still in its infancy, World Weather Online seemed to be a better fit for my current needs, although Weather Underground’s website and analytics are appealing in an aesthetically pleasing kind of way.
</p>

<p class="p1">
  Feel free to add any comments or correct anything I said in the comments. I’d also love to hear what choices other people made and what their criteria was in order to second guess myself. Additionally, I haven&#8217;t used most of these, so I cannot vouch for the accuracy of the data that is being received, only the perceived ease of use and data available.
</p>

<p class="p1">
  <strong>Update (4/2/2013): </strong>I ended up using HAMweather when I finally got around to working on this <a title="Introducing Weather Wardrobe: The quickest way to get dressed in the morning" href="http://michaelwelburn.com/2013/02/27/introducing-weather-wardrobe-the-quickest-way-to-get-dressed-in-the-morning/" target="_blank">project</a> in 2013.
</p>
