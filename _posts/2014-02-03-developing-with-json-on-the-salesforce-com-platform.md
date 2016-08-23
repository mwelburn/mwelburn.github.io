---
id: 666
title: Developing with JSON on the Salesforce.com Platform
date: 2014-02-03T06:00:15+00:00
author: Michael Welburn
guid: http://michaelwelburn.com/?p=666
permalink: /2014/02/03/developing-with-json-on-the-salesforce-com-platform/
categories:
  - Apex
  - Programming
  - Salesforce
tags:
  - Apex
  - JSON
  - Salesforce
---
As I work with different developers and existing Force.com implementations, I&#8217;ve come across quite a few implementations that leverage REST based 3rd party APIs, most often using <a title="json" href="http://www.json.org/" target="_blank">JSON</a>. While the ability to <a title="Apex Methods JSON" href="http://www.salesforce.com/us/developer/docs/dbcom_apex250/Content/apex_methods_system_json.htm" target="_blank">serialize and deserialize JSON</a> was released a year or two back, I find that developers aren&#8217;t always aware of it. Prior to this release, developers had to manually traverse and parse JSON, which ended up requiring a lot of conditional statements depending on the number of parameters involved.

<!--more-->

An example of a simple JSON string parsing is below:

    String body = '{"name" : "Michael", "company" : "Sonoma Partners"}';
    String name;
    String company;

    JSONParser parser = JSON.createParser(body);
    while (parser.nextToken() != null) {
      if ((parser.getCurrentToken() == JSONToken.FIELD_NAME)) {
        String fieldName = parser.getText();
        parser.nextToken();
        if (fieldName == 'name') {
          name = parser.getText();
        } else if (fieldName == 'company') {
          company = parser.getText();
        }
      }
    }

There are some other ways to accomplish the same feat, but regardless of the solution it can get pretty verbose…and this was only two fields on a single record. Imagine double digit fields, multiple records, and child records. The amount of parsing involved gets pretty messy. Additionally, due to the multi-tenant environment that Apex code is executing in and the [Governor Limits](http://www.salesforce.com/us/developer/docs/apexcode/Content/apex_gov_limits.htm "Governor Limits") in place to ensure that resources aren&#8217;t exploited, traversing a rather large JSON body can have a negative performance impact particularly around number of script statements being executed or even throw an error.

Leveraging the JSON methods in Apex, developers can now define an Apex object (similar to a <a title="POJO" href="http://en.wikipedia.org/wiki/Plain_Old_Java_Object" target="_blank">POJO</a> in Java) where each parameter in the JSON body maps to an attribute defined on the object. Provided this mapping is correct (i.e. field names match), deserializing the JSON body and casting it to the Apex object turns it into an instance of that object, no manual parsing necessary and in only one line of code.

    public class SonomaJSON {
      String name;
      String company;
    }

    String body = '{"name" : "Michael", "company" : "Sonoma Partners"}';
    SonomaJSON myObject = (SonomaJSON)JSON.deserialize(body, SonomaJSON.class);

One of the neater aspects of this is that it also applies to multiple records and/or children in the JSON body, which are able to be deserialized into collections of Apex objects.

This also works the other way around, allowing a user to build up an Apex object (or even an sObject defined in Salesforce.com, such as an Account or Contact) and run JSON.serialize(account) to get a JSON string of the fields. This is especially nice if you need to provide a JSON representation of Salesforce.com objects to another system or some client side javascript for displaying.

The biggest benefits of using (or converting existing manual parsing to) these Apex JSON methods are the following:

  1. Your code is much more readable, and self documents itself on what the expected JSON response/request is supposed to look like.
  2. If you need to add another field based on an API change later, it is as simple as adding another field to the Apex Object.
  3. You are simply writing significantly less code by not manually parsing&#8230;which means you also don&#8217;t have to write the unit test for it.

For additional information, take a look at the Force.com Developer page:

[http://wiki.developerforce.com/page/Getting\_Started\_with\_Apex\_JSON](http://wiki.developerforce.com/page/Getting_Started_with_Apex_JSON "Apex JSON - Getting Started")
