---
id: 491
title: Tips on Passing the Salesforce.com Advanced Developer (501) Certification Exam
date: 2013-08-27T09:00:42+00:00
author: Michael Welburn
guid: http://michaelwelburn.com/?p=491
permalink: /2013/08/27/tips-on-passing-the-salesforce-com-advanced-developer-501-certification-exam/
categories:
  - Salesforce
tags:
  - 501
  - Advanced Developer
  - certification
  - Salesforce
---
Last week I passed the first part of the Salesforce Advanced Developer (501) Certification, which was a 69 question multiple choice exam that required a 73% to pass. I still have a programming assignment and essay questions that will come (much) later this fall before I am fully certified, but this was a certification that I felt pretty compelled to complete considering I felt like my experience working on the Salesforce platform the last few years made me quite familiar with the topics covered and it would be nice to validate that.

<!--more-->

Reviewing the <a title="Developers Page" href="http://certification.salesforce.com/developers" target="_blank">study guide</a> (and many blogs written by those who have passed), I came across most of the general concepts that were covered. Some of the blogs I found valuable for preparation:

  * <a title="Class Notes" href="http://saramorgandotnet.files.wordpress.com/2013/04/apex-class-notes-spring-2012-premier-training.pdf" target="_blank">Sara Morgan&#8217;s Class Notes</a>
      * Good overview, though might be a bit basic for seasoned developers
  * <a title="Jeff Douglas" href="http://blog.jeffdouglas.com/2009/07/13/i-passed-the-salesforce-com-certified-advanced-developer-exam-so-can-you/" target="_blank">Jeff Douglas &#8211; I Passed the Salesforce.com Certified Advanced Developer Exam &#8211; So Can You!</a>
  * <a title="Matt Lacey" href="http://www.laceysnr.com/2011/12/five-oh-one-part-one-advanced-developer.html" target="_blank">Matt Lacey &#8211; Five Oh One Part One: The Advanced Developer Certification Exam</a>
  * <a title="Steven Herod" href="http://limitexception.herod.net/2011/12/14/helping-you-pass-your-501-advanced-developer-exam/" target="_blank">Steven Herod &#8211; Helping you pass your 501 Advanced Developer Exam</a>
      * Even if you don&#8217;t actually do the exercise, make a mental checklist of all the things listed and ensure that you know how to do them and when/why they are used
  * <a title="Cory Cowgill" href="http://corycowgill.blogspot.com/2011/05/passing-forcecom-advanced-developer-501.html" target="_blank">Cory Cowgill &#8211; Passing the Force.com Advanced Developer Exam</a>

After taking the test, I&#8217;d have to say that someone who works with the platform day-in and day-out should get at least half of the questions right (with the caveat that they are doing things correctly) just by drawing for your experience. Although without extra preparation there were some aspects of the platform that I either had never gotten to use before or that I needed to brush up on to be completely confident that I could correctly answer questions about them.

Some of the more obvious things to know based on the aforementioned resources if you aren&#8217;t already intimately familiar with them:

  * Know how to use the Force.com Migration Tool and Force.com IDE and what they are capable of
  * Know the <a title="Apex Code Best Practices" href="http://wiki.developerforce.com/page/Apex_Code_Best_Practices" target="_blank">best practices</a> of Apex to prevent hitting governor limits
  * Know how to write unit tests
  * Know how to write Apex Controllers and Extensions, what the constructors look like, and how Apex Controllers interact with Visualforce Pages
  * Basic SOQL
  * Team Development (and Development Lifecycle) best practices

In particular, the following were things that I paid careful attention to that I don&#8217;t work with every day:

  * Inbound and Outbound Email Services
  * Visualforce Page Templates and Visualforce Email Templates
  * Custom Components
  * Visualforce Action Tags
  * Apex Annotations and Webservices
  * Dynamic Apex
  * Methods to access Schema
  * Trigger and Visualforce Page execution orders

Remember that anything in the official study guide is fair game, so I would suggest that if you don&#8217;t have hands on experience that you at least go through some examples instead of just trying to memorize anything. If you come from a Java/C# background, you have a leg up in that you will be pretty familiar with the syntax, but I still wouldn&#8217;t advocate jumping into the test without some test development. I&#8217;m always pushing the Apex and Visualforce <a title="Force.com Workbook" href="http://wiki.developerforce.com/page/Force.com_workbook" target="_blank">Force.com Workbooks</a> as a great tool to jump into the programming side of the platform.

As many other bloggers have noted, if you simply read the <a title="Apex Developer Guide" href="http://www.salesforce.com/us/developer/docs/apexcode/salesforce_apex_language_reference.pdf" target="_blank">Apex Developer Guide</a>, <a title="Visualforce Developer Guide" href="http://www.salesforce.com/us/developer/docs/pages/salesforce_pages_developers_guide.pdf" target="_blank">Visualforce Developer Guide</a>, <a title="Migration Tool Guide" href="http://www.salesforce.com/us/developer/docs/daas/salesforce_migration_guide.pdf" target="_blank">Migration Tool Guide</a>, and <a title="Development Lifecycle Guide" href="http://www.salesforce.com/us/developer/docs/dev_lifecycle/salesforce_development_lifecycle.pdf" target="_blank">Development Lifecycle Guide</a>, you will know everything asked on the test. Unfortunately that isn&#8217;t always possible, so ensuring that you have strong fundamentals on the platform and augmenting that with some of the pieces that you haven&#8217;t used before that are referenced above should help you pass the test. Worst case, you learn some new techniques and/or unveil some functionality that you can integrate in your code going forward!

_For more information on the second part of the Advanced Developer certification, take a look at the followup blog post: <a title="Salesforce.com Advanced Developer (501) Programming Assignment & Essay" href="http://michaelwelburn.com/2014/01/30/salesforce-com-advanced-developer-501-programming-assignment-essay/" target="_blank">Salesforce.com Advanced Developer (501) Programming Assignment & Essay</a>._
