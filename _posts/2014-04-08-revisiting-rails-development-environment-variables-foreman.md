---
id: 859
title: Setting up Rails Environment Variables with Foreman
date: 2014-04-08T21:55:23+00:00
author: Michael Welburn
layout: post
guid: http://michaelwelburn.com/?p=859
permalink: /2014/04/08/revisiting-rails-development-environment-variables-foreman/
categories:
  - Programming
  - Ruby on Rails
tags:
  - Foreman
  - Procfile
  - Ruby on Rails
---
When I started trying to learn Ruby on Rails, along with deploying my fun projects to Heroku, I ran into the issue of <a title="Setting Rails Environment Variables Outside of Version Control" href="http://michaelwelburn.com/2012/04/30/setting-rails-environment-variables-outside-of-version/" target="_blank">how to set environment variables locally</a> in a way that didn&#8217;t require any overhead from me each time I opened a new terminal window. At the time, I settled on creating a separate ruby file that I loaded as part of the application initialization. Unfortunately, I ran into an issue going down this path recently that led me to reconsider that practice.

<!--more-->

## Setup

In this particular case, I was using the popular <a title="Paperclip" href="https://github.com/thoughtbot/paperclip" target="_blank">paperclip</a> gem to upload images to Amazon S3. The setup is pretty simple, you can set some default values for authentication in your application.rb or environment ruby file, and then you can override those directly in the model. In my case, I wanted to test with S3 even locally on my computer, so I place the configuration in application.rb.

    config.paperclip_defaults = {
      :storage => :s3,
      :s3_credentials => {
        :bucket => ENV['S3_BUCKET_NAME'],
        :access_key_id => ENV['AWS_ACCESS_KEY_ID'],
        :secret_access_key => ENV['AWS_SECRET_ACCESS_KEY']
      }
    }

This required me to have some dynamic environment variables so the code wouldn&#8217;t need to be modified between development and production. I went with the same practice as I mentioned in the opening paragraph, creating a ruby file and loading it prior to the application initialization.

    ENV['S3_BUCKET_NAME'] ||= 'MyProject_DEV'
    ENV['AWS_ACCESS_KEY_ID'] ||= 'XXXXXXXX'
    ENV['AWS_SECRET_ACCESS_KEY'] ||= 'YYYYYYYY'

## The Problem

While everything worked fine in production, I was running into issues locally. The actual error I was getting was the following:

> missing required :bucket option

The error itself is quite blunt, and is referring to the :bucket field in my configuration not being set. I googled the error and found a handful of people running into the same issue, but outside of the obvious forgetting to set that value, there wasn&#8217;t much help. I was quite confident the environment variable was being set, so I ignored that advice and started debugging.

## Troubleshooting

I spun up the rails console to spit out the environment variable for S3\_BUCKET\_NAME. Sure enough, it was set. I then decided to add some **puts** statements in my code to log whether the environment variables were being loaded before the application.rb was run in the terminal.

While printing out **Paperclip::Attachment.default_options**, I started to notice that my configurations were in fact not being set in the application.rb. Strangely, even though the custom ruby file that was setting the environment variables was finishing before the logging I set up in application.rb started, they weren&#8217;t persisted as part of that file&#8217;s invocation.

_Note that in my previous blog post, the environment variables I was setting were not used as part of the initialization. It is quite possible I would have run into the same situation back then if that were the case._

<span style="font-size: 1.5em; line-height: 1.5em;">The Solution</span>

After spending a bit of time debugging, I started looking around a little more at whether there was a better practice. This led me back to <a title="foreman" href="https://github.com/ddollar/foreman" target="_blank">foreman</a>, a tool that I believe is packaged as part of the rails install, and something that I disregarded when I first started with rails.

Foreman lets you set up a handful of tasks to run with a single command. In some of my previous projects, this might have encompassed starting up a Postgres database and a Redis Server as part of the startup of the application server (instead of running them on my computer all the time, even when working on other things). On an upcoming project I am going to be using <a title="resque" href="https://github.com/resque/resque" target="_blank">resque</a> for background jobs, and foreman will allow me to start those jobs up at the same time as my local application server. One of the features that foreman also brings to the table is the ability to set environment variables.

I found this <a title="Foreman and Environment Variables" href="http://mauricio.github.io/2014/02/09/foreman-and-environment-variables.html" target="_blank">blog post</a> helpful as I was wrapping my head around it. In my very basic case, I was initially concerned with setting environment variables; the rest could be added later. To set that up, you create a file called **.env** at your project root and use the **key=value** syntax for your environment variable declaration (without quotation marks).

    S3_BUCKET_NAME=MyProject_DEV
    AWS_ACCESS_KEY_ID=XXXXXXXX
    AWS_SECRET_ACCESS_KEY=YYYYYYYY

Then you set up a file called **Procfile** at the same project root directory. This is typically where you would start up different processes, but in my case I only wanted to start my rails server.

    web: bundle exec rails server -p 3000 -e development

Once those two files are set up, from your command line you can run **foreman start** to kick it off.

While having to debug the issue above was rather annoying, I&#8217;m glad that it led me to rediscovering foreman now that some of my current side projects are finding ways to make use of it more than some of my entry level projects of the past. I&#8217;m looking forward to setting it up as a standard going forward.