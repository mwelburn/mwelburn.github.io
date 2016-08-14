---
id: 18
title: Setting Rails Environment Variables Outside of Version Control
date: 2012-04-30T03:26:00+00:00
author: Michael Welburn
layout: post
guid: http://michaelwelburn.com/2012/04/30/setting-rails-environment-variables-outside-of-version/
permalink: /2012/04/30/setting-rails-environment-variables-outside-of-version/
tumblr_br0nc080_permalink:
  - http://br0nc080.tumblr.com/post/22102510702/setting-rails-environment-variables-outside-of-version
tumblr_br0nc080_id:
  - 22102510702
categories:
  - Programming
  - Ruby on Rails
tags:
  - API
  - config-vars
  - development
  - environment variable
  - heroku
  - rails
redirect_from:
  - /post/22102510702/setting-rails-environment-variables-outside-of-version/
  - /post/22102510702/
---
_**UPDATE (4/8/2014):** I&#8217;ve posted a newer <a title="Setting up Rails Environment Variables with Foreman" href="http://michaelwelburn.com/2014/04/08/revisiting-rails-development-environment-variables-foreman/" target="_blank">blog post around using foreman</a> as opposed to this method that alleviated an issue I was having with environment variables that were needing during initialization._

In the past, I’ve always fooled around with different API’s and committed those keys into version control systems, considering I was the only person working on the project. More specifically, I’m been leveraging OmniAuth with the <a title="devise" href="https://github.com/plataformatec/devise" target="_blank">devise</a> gem in order to allow Facebook authentication, and hardcoding the API key and secret in the devise.rb file using the particularly fancy Rails.env.production? condition to determine whether I am using my production or developer set of API key and secret.

However, recently I’ve needed to keep the API keys secret, and frankly was not entirely sure how to do it in Rails. After realizing no one else I asked knew the solution, I decided I needed to figure it out.

<!--more-->

The easy part is deploying to Heroku. Heroku allows you to set <a href="https://devcenter.heroku.com/articles/config-vars" target="_blank">config-vars</a>, which are environment level variables that your application can reference. In this sense, I can set something up like:

    heroku config:add FACEBOOK_KEY=1111111111111 FACEBOOK_SECRET=aaaaaaaaaaaaaaaa

This allows my application to access those variables using ENV[‘FACEBOOK_KEY’]. However, the more interesting piece was trying to find out the best way to store my developer API keys (the ones pointing to localhost:3000). Heroku suggests using Foreman, which basically ensures that your environment variables are set up using a Procfile. I poked around a little bit but being rather new to Rails and not needing to do anything fancy like setting up background processes at this time, found it a little bit confusing for the simple task at hand. I ended up stumbling upon this <a href="http://tammersaleh.com/posts/managing-heroku-environment-variables-for-local-development" target="_blank">blog</a> which detailed a pretty reasonable way to set up the developer side of environment variables.

The gist of it is that you create a file (in this case, heroku_env.rb) that defines your environment variables by adding lines such as:

    # This file contains the ENV variables to access Facebook and Twitter. These are for localhost,
    # and should NOT be committed anywhere. The commented out values are for heroku production, which
    # are set up using config-vars
    ENV['FACEBOOK_KEY']    = "1111111111111"
    ENV['FACEBOOK_SECRET'] = "aaaaaaaaaaaaaaaa"

I added this file into /config, and it does not get committed to version control (hence your API keys stay a secret), so I added it to .gitignore.

In the environment.rb file, you add the following snippet of code:

    # Load the rails application
    require File.expand_path('../application', __FILE__)
    #Load heroku vars from local file
    heroku_env = File.join(Rails.root, 'config', 'heroku_env.rb')
    load(heroku_env) if File.exists?(heroku_env)
    # Initialize the rails application
    TestApp::Application.initialize!

What this does is load the environment variables from your local file (if it exists), otherwise it does not load anything — which is exactly what we want to happen on Heroku, since we have already set the environment variables up via the heroku config. Now when someone else wants to fork the code, all they have to do is create their own heroku_env.rb file and they’ll easily be able to use their own API keys.
