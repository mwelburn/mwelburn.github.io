---
id: 467
title: 'Error Code 155037: Problem Switching from SVN 1.6.17 to 1.7.8 on OS X'
date: 2013-08-08T09:00:32+00:00
author: Michael Welburn
layout: post
guid: http://michaelwelburn.com/?p=467
permalink: /2013/08/08/error-code-155037-problem-switching-from-svn-1-6-17-to-1-7-8-on-os-x/
categories:
  - Programming
tags:
  - 155037
  - svn
---
Recently I noticed that <a title="Versions" href="http://versionsapp.com/" target="_blank">Versions</a>, the OS X specific visual SVN tool that I&#8217;ve used for many years (and love), was set to the check out repositories with 1.6.17 version of SVN. While some people might not care, there is one (in my opinion, huge) update between 1.6.17 and 1.7.8 that I really wanted, which was to have all of the hidden .svn folders that show up in every single folder in the hierarchy of a checked out repository removed in lieu of a single folder at the root with all the metadata. I&#8217;d long struggled with different IDEs working with Salesforce development, which sometimes refresh entire folders (that I often had under version control) which subsequently deletes the .svn folder as well, creating issues that I had to resolve manually.

<!--more-->

[<img class="aligncenter size-full wp-image-468" alt="Screen Shot 2013-08-07 at 10.43.42 PM" src="http://michaelwelburn.com/wp-content/uploads/2013/08/Screen-Shot-2013-08-07-at-10.43.42-PM.png" width="442" height="523" srcset="http://michaelwelburn.com/wp-content/uploads/2013/08/Screen-Shot-2013-08-07-at-10.43.42-PM.png 442w, http://michaelwelburn.com/wp-content/uploads/2013/08/Screen-Shot-2013-08-07-at-10.43.42-PM-253x300.png 253w" sizes="(max-width: 442px) 100vw, 442px" />](http://michaelwelburn.com/wp-content/uploads/2013/08/Screen-Shot-2013-08-07-at-10.43.42-PM.png)

After changing my preferences in Versions, I thought everything was good to go. That is, until I started receiving the following error after every successful commit:

<img class="aligncenter size-full wp-image-473" alt="Screen Shot 2013-08-08 at 1.37.27 PM" src="http://michaelwelburn.com/wp-content/uploads/2013/08/Screen-Shot-2013-08-08-at-1.37.27-PM.png" width="513" height="296" srcset="http://michaelwelburn.com/wp-content/uploads/2013/08/Screen-Shot-2013-08-08-at-1.37.27-PM.png 513w, http://michaelwelburn.com/wp-content/uploads/2013/08/Screen-Shot-2013-08-08-at-1.37.27-PM-300x173.png 300w" sizes="(max-width: 513px) 100vw, 513px" />

This error message was rather cryptic, and gave me absolutely no help (nor did searching the internet). I tried many different things, including manually going into the checked out SVN repository to try to run clean up, but nothing resolved it. However, when I tried to clean up the repository, it gave me an error about my SQLite library being too old. I proceeded to update that with <a title="Homebrew" href="http://brew.sh/" target="_blank">homebrew</a>, but then got another error:

> Error code 155037: Error bumping revision post-commit

Interestingly (or, as you may infer from the error message), the commits did succeed, but there was an issue locally incrementing the revision number. Unfortunately, this seemingly innocent message also meant that the .svn folder somehow got corrupted, as Versions could not even display the folder hierarchy anymore. My only real course of action was to delete the repository and re-check it out each time (saving my Force.com/Eclipse project settings), which took me doing twice before I wanted to punch something. I had to stop doing actual work and figure out how to resolve this nightmare.

After doing a bit of research and poking around, I finally came across someone else who experienced the same problem in the post <a title="Problems with SVN in Eclipse on Mac OS X Mountain Lion (10.8)" href="http://benohead.com/problems-with-svn-in-eclipse-on-mac-os-x-mountain-lion-10-8/" target="_blank">Problems in SVN in Eclipse on Mac OS X Mountain Lion (10.8)</a>. In a nutshell, it appears that after I first upgraded SQLite to work with the newer version of SVN, the location that SVN was expecting to find the SQLite library did not exist. What I needed to do was the last two steps of the linked post, which was to locate any references to this particular file, and then create a symbolic link from an actual library to the expected location.

&nbsp;