---
id: 724
title: A Better Way to Use Test.isRunningTest() in Apex
date: 2014-03-03T06:00:47+00:00
author: Michael Welburn
layout: post
guid: http://michaelwelburn.com/?p=724
permalink: /2014/03/03/a-better-way-to-use-test-isrunningtest-in-apex/
categories:
  - Apex
  - Programming
  - Salesforce
tags:
  - Apex
  - Salesforce
  - Unit Testing
---
I haven&#8217;t leveraged <a title="Test.isRunningTest" href="http://www.salesforce.com/us/developer/docs/dbcom_apex/Content/apex_System_Test_isRunningTest.htm" target="_blank">Test.isRunningTest()</a> in Apex, as I have yet to find a use case in my code where it was better to split up functionality to increase testability or use <a title="Http Callout Mock" href="http://www.salesforce.com/us/developer/docs/apexcode/Content/apex_classes_restful_http_testing_httpcalloutmock.htm" target="_blank">mock callouts</a>, but across a larger project I&#8217;m working on I&#8217;ve noticed that method being used reasonably often to set expected response values from outbound HTTP requests to allow the code to continue running smoothly in tests. Unfortunately, that means the error handling aspects of that code base is impossible to unit test. This got me thinking about what a better practice might be.

<!--more-->

From the <a title="How to Write Good Unit Tests" href="http://wiki.developerforce.com/page/How_to_Write_Good_Unit_Tests" target="_blank">developerforce wiki</a>:

> There are some situations, when you will just not be able to test some code in a normal matter &#8211; eg. you are using objects that cannot be inserted in tests, or doing some http requests. In such case, if you think there is no other option, consider using Test.isRunningTest(). This static method allows you to discover whether the code was run from test method. Therefore, for example you might:
> 
>   * return hardcoded String instead of calling http request and parsing the body
>   * return a fixed array of objects from a method

An example of that might be the following snippet of code that would be inside a method:

    Boolean isError = someMethodThatParsesResponse(response);
    if (Test.isRunningTest())
    {
      isError = false;
    }
    if (isError)
    {
      // alert the user there is a problem, and do some cleanup
      ...
    }

In this particular case, the unit tests will never get into the error condition **if (isError)** because that variable is explicitly being forced to true in all unit tests. What I&#8217;m proposing is that we leverage a static boolean for unit testing to allow the concept of Test.isRunningTest() to be configurable.

    public class TestUtils
    {
      public static Boolean enable_isRunningTest = true;
    
      public static Boolean isRunningTest()
      {
        return Test.isRunningTest() && enable_IsRunningTest;
      }
    }

By leveraging **TestUtils.isRunningTest()** instead of Test.isRunningTest() in your Apex, you gain a little bit more flexibility for testing. While your end to end code that requires a lot of hardcoded response values still can work as intended, you can individually test your calls and set **TestUtils.enable_isRunningTest = false** in specific unit tests in order to see the failures happen. This will give you a little more control over behavior, although in this particular instance mock callouts would remain the best bet.

If you have any other best practices that you&#8217;ve leveraged in the past, I&#8217;d love to hear about them!

**Note:** I suspect the biggest issue preventing people from using mock callouts (myself included in the past) is the lack of knowledge around how to test more than 1 callout in a single test execution. Fortunately, there is a way to accomplish this using <a title="Http Testing Static" href="http://www.salesforce.com/us/developer/docs/apexcode/Content/apex_classes_restful_http_testing_static.htm" target="_blank">MultiStaticResourceCalloutMock</a> (about halfway down the link) that lets you provide a Static Resource of responses and set up which endpoints in your unit test you want to return specific responses.