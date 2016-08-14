---
id: 231
title: Leveraging Redis with Ruby on Rails and Heroku
date: 2013-03-04T07:00:12+00:00
author: Michael Welburn
layout: post
guid: http://michaelwelburn.com/?p=231
permalink: /2013/03/04/leveraging-redis-with-ruby-on-rails-and-heroku/
categories:
  - Programming
  - Ruby on Rails
  - Weather Wardrobe
tags:
  - heroku
  - homebrew
  - redis
  - Ruby on Rails
---
There are a lot of benefits to leveraging third party APIs to retrieve data that makes your application more valuable. In the case of <a title="WeatherWardrobe" href="http://www.weatherwardrobe.com" target="_blank">Weather Wardrobe</a>, I make use of <a title="HAMweather" href="http://www.hamweather.com" target="_blank">HAMweather</a>&#8216;s API to retrieve weather forecast information based on zip codes. This saves me the effort of generating the content myself (which I am in no position to do), or scraping a weather website to grab the information displayed on a page.

However, with APIs come limitations, in this case a limit on the number of API calls (both daily as well as per minute) based on the package that is purchased by the developer. In the case of the free tier, making more than 750 API calls a day or 15 API calls a minute is not allowed unless I want to start paying. While the notion of having 750 unique visitors seems pretty farfetched for a fun project, it gave me an excuse to do some simple caching, which is where <a title="redis" href="http://redis.io/" target="_blank">Redis</a> comes into play.

### <!--more-->

### Redis

Redis is an advanced key-value store that allows you to quickly store and retrieve information in memory. I took a look at <a title="Memcached" href="http://memcached.org/" target="_blank">Memcached</a> as well, but it seemed like Redis has overtaken it in recent years in terms of ease of use.

One of the nice things about both of the options is their availability on Heroku as <a title="Redis To Go Add-on" href="https://addons.heroku.com/redistogo" target="_blank">add-ons</a>, limiting the amount of effort I have to spend getting different applications integrated. With Redis, I merely referenced an <a title="Redis To Go" href="https://devcenter.heroku.com/articles/redistogo" target="_blank">article</a> on Heroku and was up an running pretty quickly.

> heroku addons:add redistogo

Locally, I used <a title="homebrew" href="http://mxcl.github.com/homebrew/" target="_blank">homebrew</a> to install redis and followed the installation instructions provided in the terminal window.

> brew install redis

Further expediting development was the ability to leverage the Redis ruby <a title="Redis gem" href="https://github.com/redis/redis-rb" target="_blank">gem</a>. Between the <a title="Redis gem overview" href="https://github.com/redis/redis-rb/wiki/Redis-rb-Overview" target="_blank">overview</a> of commands available to the gem, I figured out that I merely needed to leverage get, set, and expire.

The use case that I implemented in this initial offering was caching requests for the same zip code within the same core hours. By default, HAMweather provides forecasts for 12 hour blocks (6am to 6pm), and while those forecasts may change, when I am pulling the forecasts the day of I can be pretty confident they&#8217;ll stay pretty close. Because of this, I can speed up the user experience for all of the subsequent users who are interested in the weather in the same zip code as someone else by bypassing interaction with the HAMweather service, and simply relaying the same response the first user received.

Once Redis is populated with hashes of forecasts of zip codes, I need to make sure that when the forecast is no longer needed, I expire the values. To do this, I am able to take the timestamp HAMweather gives me as the valid date and add 12 hours to it.

    # config/initializers/edis.rb
    uri = URI.parse(ENV['REDISTOGO_URL'])
    $redis = Redis.new(:host => uri.host, :port => uri.port, :password => uri.password)

    if result = $redis.get(@zipcode)
       forecast = JSON.parse(result)
    else
       url = "https://api.aerisapi.com/forecasts/#{@zipcode}?from=today&#{client_params}"
    end
    
    ...
    
    if url
       $redis.set(@zipcode, forecast.to_json)
       #expire 12 hours after it is valid
       expiration_time = dateTimeISO.to_i + 12.hours
       $redis.expireat(@zipcode, expiration_time)
    end

This is arguably the simplest use case possible for Redis, as there is no persistence, only one page, and no concurrency issues, but it shows how simple it is to get something like this off the ground.

### Notes

I did have a couple of interesting things pop up as I was implemented Redis. The first was that, coming from a Java background, I am used to all time being done in milliseconds. I now have learned that Ruby seems to instead use seconds as the count, which had me a bit confused for a while.

I also had an issue deploying my application to Heroku after implementing Redis.

> Running: rake assets:precompile rake aborted! bad URI(is not URI?):
  
> &#8230;&#8230;&#8230;
  
> Tasks: TOP => environment (See full trace by running task with &#8211;trace)
  
> Precompiling assets failed, enabling runtime asset compilation
  
> Injecting rails31\_enable\_runtime\_asset\_compilation
  
> Please see this article for troubleshooting help: http://devcenter.heroku.com/articles/rails31\_heroku\_cedar#troubleshooting

After researching a bit online, I found an <a title="Heroku Tip with Redis" href="http://blog.nathanhumbert.com/2012/01/rails-32-on-heroku-tip.html" target="_blank">article</a> that enlightened me as to what the issue was. Essentially, by putting my Redis instantiation in the initializers folder, it was trying to connect to Redis on startup during asset complication. Unfortunately with Heroku, the services have not started up yet at that point, so it was failing to connect. To alleviate that issue, I added the following line to the application.rb file.

    config.assets.initialize_on_precompile = false