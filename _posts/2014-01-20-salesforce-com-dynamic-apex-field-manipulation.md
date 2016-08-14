---
id: 628
title: Salesforce.com Dynamic Apex Field Manipulation
date: 2014-01-20T06:00:06+00:00
author: Michael Welburn
layout: post
guid: http://michaelwelburn.com/?p=628
permalink: /2014/01/20/salesforce-com-dynamic-apex-field-manipulation/
categories:
  - Apex
  - Programming
  - Salesforce
tags:
  - Apex
  - Salesforce
---
I often get the idea that most people assume Apex is <a title="What is Apex?" href="https://www.salesforce.com/us/developer/docs/apexcode/Content/apex_intro_what_is_apex.htm" target="_blank">always strongly typed</a>. However, you might not know that you can access fields on an sObject by using a different notation than <a title="Dot Notation" href="http://worldgame.blogspot.com/2005/06/dot-notation.html" target="_blank">dot notation</a>. This is particularly useful when you are working with some sort of [Dynamic Apex](http://www.salesforce.com/us/developer/docs/apexcode/Content/apex_dynamic.htm "Dynamic Apex") or building a package that has to be installed in environment where features like Record Types may or may not be enabled. I ended up coming across a separate use case while trying to extend an controller in an ISV&#8217;s unmanaged package.

<!--more-->

The problem that I had was that I needed to access one of the ISV’s Apex controller&#8217;s “get” methods where they were rendering a pageBlockTable over a collection of a custom Apex object (an Apex class wrapper of data, rather than an sObject). Unfortunately, they declared this Apex object inside of their class similar to the following:

    public class TestController
    {
      // …implementation here…
    
      public List<TestObject> getTestObjects()
      {
        // …get collection...
      }
    
      class TestObject
      {
        public String myField;
        // …object here...
      }
    }

The disappointing part here (for me) is that by not defining that TestObject class as public, no other classes can access it. That left me out of luck trying to simply reference it from my own controller as a strongly typed collection, as Salesforce refused (rightly) to compile. While some might think that, since it is an unmanaged package, that I should just change that class reference to public, that really isn&#8217;t an option. This ISV would update this file whenever they had any internal changes of note, so if I made a change it would get overwritten at some point (having them update their code was unfortunately also not an option).

At that point, the only thing left was to use a generic reference to that Apex class by calling it a generic Object.

    public class MyController
    {
      public List<Object> getTestObjects()
      {
        TestController tc = new TestController();
        return tc.getTestObjects();
      }
    }

By returning a collection of generic objects, I no longer could use dot notation. Instead, I had to use string keys to access values, which implies that I have to actually know the names of the fields. Fortunately, I did have this information due to the Visualforce page that the ISV leveraged. In order to leverage this dynamic retrieval of fields, you can leverage the Apex method <a title="sObject get()" href="http://www.salesforce.com/us/developer/docs/apexcode/Content/apex_System_SObject_get.htm" target="_blank">obj.get(&#8216;fieldName&#8217;)</a>.

    MyController mc = new MyController();
    for (Object obj : mc.getTestObjects())
    {
      // Does not compile - obj.myField
    
      // Works
      System.debug(‘myField value: ‘ + obj.get('myField'));
    }

The bracket notation obj[&#8216;fieldName&#8217;] is the equivalent that can be used on Visualforce pages. ****

    <apex:repeat value=“{!testObjects}” var=“obj”>
      {!obj[‘myField’]}
    </apex:repeat>

Additionally, while I didn&#8217;t need to use it in this case, there is a <a title="sObject .put()" href="http://www.salesforce.com/us/developer/docs/apexcode/Content/apex_System_SObject_put.htm" target="_blank">.put()</a> method that sets the value of a field with a dynamic name as well.

    Account acct = new Account();
    // Explicit put method call
    acct.put('Name', 'Test Account');
    insert acct;