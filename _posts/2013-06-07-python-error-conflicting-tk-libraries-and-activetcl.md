---
id: 409
title: 'Python Error: Conflicting Tk Libraries and ActiveTcl'
date: 2013-06-07T08:00:22+00:00
author: Michael Welburn
layout: post
guid: http://michaelwelburn.com/?p=409
permalink: /2013/06/07/python-error-conflicting-tk-libraries-and-activetcl/
categories:
  - Programming
  - Python
tags:
  - ActiveTcl
  - python
  - Tcl
  - Tk
---
The other day I ran across an issue running some python code on a Mac (OSX 10.8) in a virtual machine I had spun up solely for testing. This was the error I was receiving:

    objc[98483]: Class TKApplication is implemented in both /Library/Frameworks/Tk.framework/Versions/8.5/Tk and /System/Library/Frameworks/Tk.framework/Versions/8.5/Tk. One of the two will be used. Which one is undefined.
    objc[98483]: Class TKMenu is implemented in both /Library/Frameworks/Tk.framework/Versions/8.5/Tk and /System/Library/Frameworks/Tk.framework/Versions/8.5/Tk. One of the two will be used. Which one is undefined.
    objc[98483]: Class TKContentView is implemented in both /Library/Frameworks/Tk.framework/Versions/8.5/Tk and /System/Library/Frameworks/Tk.framework/Versions/8.5/Tk. One of the two will be used. Which one is undefined.
    objc[98483]: Class TKWindow is implemented in both /Library/Frameworks/Tk.framework/Versions/8.5/Tk and /System/Library/Frameworks/Tk.framework/Versions/8.5/Tk. One of the two will be used. Which one is undefined.
    Segmentation fault: 11
    <!--more-->

As background, when I had installed python 2.7.5 (or so I thought) on this virtual machine, I got some errors when installing other packages that Tk was not installed. In hindsight, I had installed a faulty package of python or something, but at the time I started Googling to see whether there was an issue, and the internet led me to believe that python on a Mac <a title="http://www.python.org/download/mac/tcltk/" href="http://www.python.org/download/mac/tcltk/" target="_blank">didn&#8217;t have Tk support natively</a>. I proceeded to install it (and then later realized my error and installed python 2.7.5 correctly), but didn&#8217;t think anything of the ActiveTcl install because it wasn&#8217;t hurting anything. Unfortunately later on, it did.

While searching the internet wasn&#8217;t that much help, it did help me isolate that the culprit was likely ActiveTcl causing library conflicts. The official <a title="http://docs.activestate.com/activetcl/8.5/at.install.html#uninstall" href="http://docs.activestate.com/activetcl/8.5/at.install.html#uninstall" target="_blank">documentation</a> for ActiveTcl references an uninstall script (look at the note for Snow Leopard and above, its not in the same location as what they refer to) doesn&#8217;t seem to actually do anything except tell you &#8220;uninstall file_urls&#8221;.

In order to get the libraries to stop conflicting, I took a look at both folder paths to determine which was the original and which was installed later with ActiveTcl. Based on what I found, the /Library path was the newer one, so I went with that as the folder to remove. I proceeded to run the following commands:

    cd /Library/Frameworks
    rm -r Tk.framework
    rm -r Tcl.framework

While I am not 100% that this removed everything (probably not), it did stop the conflicts from occurring and let me continue working.