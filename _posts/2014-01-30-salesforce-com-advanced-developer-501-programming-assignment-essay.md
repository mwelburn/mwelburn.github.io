---
id: 687
title: 'Salesforce.com Advanced Developer (501) Programming Assignment &#038; Essay'
date: 2014-01-30T06:00:29+00:00
author: Michael Welburn
guid: http://michaelwelburn.com/?p=687
permalink: /2014/01/30/salesforce-com-advanced-developer-501-programming-assignment-essay/
categories:
  - Apex
  - Programming
  - Salesforce
tags:
  - 501
  - Advanced Developer
  - certification
  - Salesforce
---
Back in August I <a title="Tips on Passing the Salesforce.com Advanced Developer (501) Certification Exam" href="http://michaelwelburn.com/2013/08/27/tips-on-passing-the-salesforce-com-advanced-developer-501-certification-exam/" target="_blank">posted</a> about my takeaways from the Salesforce.com <a title="Developer Certification Path" href="http://certification.salesforce.com/developers" target="_blank">Advanced Developer (501) Certification</a>&#8216;s multiple choice exam. Since then, I completed the programming and essay portions of the exam back in November, and just found out on Tuesday (~8 weeks after the due date) that I passed, leaving the fabled <a title="Technical Architect" href="http://certification.salesforce.com/architects" target="_blank">Technical Architect</a> certification as the missing trophy from my collection.

[<img class="aligncenter size-full wp-image-800" alt="cert_adv_dev_rgb" src="http://michaelwelburn.com/wp-content/uploads/2014/01/cert_adv_dev_rgb.png" width="628" height="553" srcset="http://michaelwelburn.com/wp-content/uploads/2014/01/cert_adv_dev_rgb.png 628w, http://michaelwelburn.com/wp-content/uploads/2014/01/cert_adv_dev_rgb-300x264.png 300w" sizes="(max-width: 628px) 100vw, 628px" />](http://michaelwelburn.com/wp-content/uploads/2014/01/cert_adv_dev_rgb.png)

Below are my thoughts about the second half of the certification experience.

<!--more-->

## Signup

The signup for the programming assignment is something that you&#8217;ll want to set a reminder for, as it usually &#8220;sells out&#8221; in a couple hours (and since it only comes around every 3-4 months, that is a long wait). At the time of sign up, you&#8217;ll pick a time slot to take your essay. While it is preferred that you finish your programming assignment first, you can set up the essay for any time within the month. I set mine up near the end of the month but as I was finishing early I moved the date up and took it early.

## Programming Assignment

As many others have noted, the programming assignment mentioned that it would take at least 20 hours. I ended up spending about 25-30 hours to make sure that I caught everything and was using the most ideal techniques for each situation. From talking to others who have taken the exam in the past, people spend anywhere between 20-60 hours on this part, depending on the speed at which you work and the level of comfort you have with Apex and Visualforce.

I found the content of the programming assignment pretty reasonable. Without going into details, the assignment consisted of a Trigger, Visualforce page, and Apex controller. I found that most of what I used were common to everyday Force.com development, but I did unearth a couple of techniques while researching to find the most ideal implementation that I hadn&#8217;t known about in the past (things that were harped on frequently in the multiple choice part of the exam).

Of course, unit tests are also quite important, and I probably went a little beyond what I normally would do on a project as the requirements of the assignment were pretty explicit. I had to reign myself back a bit overall on trying to implement the perfect solution to every potential scalability issue and fall back on the instructions of only implementing what is explicitly stated.

I found that I ended up being comfortable enough to state any assumptions that I was making throughout the implementation in comments around my code, explained my reasoning for choosing a particular way to implement something, and hoped those were taken into consideration.

## Essay

To be relatively blunt, if you complete (or are close to completing) your programming assignment by the time you take the essay, and you have a compelling reason for why you did things the way you did, the essay will not be an issue. Just make sure that you are familiar with your code and design decisions (I set up my essay a few days after I was code complete, so I needed a refresher just before I took it).

While you have the option of taking the essay at any time, if you try to take it before you start to work on your actual implementation, you are not going to do very well. My assumption (similar to <a title="Keir Bowden: 501 Exam" href="http://bobbuzzard.blogspot.com/2011/12/certified-forcecom-advanced-developer.html" target="_blank">Keir Bowden</a>) is that this is just an opportunity for you to prove that you actually did the assignment, and you have a good explanation for why things work the way they do.

As an aside, I had a lot of <a title="Known Issues" href="http://www.kryteriononline.com/support/known_issues/index.asp?cate=1" target="_blank">issues</a> getting my at-home proctored test to work on a Mac. For whatever reason, <a title="Webassessor" href="https://www.webassessor.com" target="_blank">Webassessor</a>&#8216;s support for Mac is something like 10.6 (which is probably 3-4 years old) and when the Sentinel program would load, it refused to load my webcam even though I followed the explicit steps they provided. I finally got it to work after having to miss my first window debugging and calling into their tech support. Hopefully that has been fixed by now, it was a pretty stressful afternoon. Word of advice to Kryterion: allow me to test my webcam working with your software before the test starts.

## Results

<p style="text-align: center;">
  <a href="http://michaelwelburn.com/wp-content/uploads/2014/01/Screen-Shot-2014-01-28-at-6.13.36-PM.png"><img alt="Advanced Developer Email" src="http://michaelwelburn.com/wp-content/uploads/2014/01/Screen-Shot-2014-01-28-at-6.13.36-PM.png" width="542" height="189" /></a>
</p>

<p style="text-align: left;">
  I ended up finding out my results about a week after some of my colleagues, which kept me on the edge of my seat (and constantly checking my email). The email itself had a small blurb about how I met the requirements, and then listed the strengths and weaknesses of my implementation (at a very high level). While it would have been nice to get an in depth report on each bullet (particularly a specific example of the weakness cited), it understandably isn&#8217;t part of the scorer&#8217;s job description.
</p>

## Summary

I found this portion of the certification exam to be a pretty good representation of what a Force.com Developer would do on a normal project (and while by far the most time consuming, the most fun). Unlike some of the really abstract concepts that you find in some of the multiple choice exams that seemingly test the extremes of your Salesforce.com knowledge and reading, nothing in this part of the assignment deviated too much from things that I&#8217;ve done for clients in the past.

I highly suggest that anyone who has been working on the platform for a year or two take a stab at this certification, as the worst case scenario is that you&#8217;ll learn some new techniques that you may not have come across before. That being said, the complete process of this certification spanned 5 months since I passed the multiple choice part (late August to late January), so make sure you identify the programming assignment windows and don&#8217;t miss out or it could be a long wait.
