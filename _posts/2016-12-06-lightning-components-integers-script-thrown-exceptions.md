---
title: Lightning Components, Integers, & Script-Thrown Exceptions
date: 2016-12-06T23:41:06+00:00
author: Michael Welburn
permalink: /2016/12/06/lightning-components-integers-script-thrown-exceptions/
twitterCardType:
  - summary
categories:
  - Salesforce
excerpt: "Debugging a Lightning Component error when using Integers as server-side parameters."
---

Working with Lightning Components, I always am prepared to come across cryptic error messages. Over time, you start to get a good idea of the regular mistakes you make that generic very unhelpful system errors and can isolate a few places to look right away -- My most frequent error is typing a semicolon instead of a comma building up JSON for server-side calls. Without a compiler for Javascript, simple mistakes tend to be a little harder to debug, and the current Salesforce error messaging is not always helpful.

However, I can across a rather interesting error earlier this week. To simplify the use case, I was working on a component that let the property editor dictate an Integer that would be used to set the LIMIT of a SOQL statement to return Account records. Below are the relevant code snippets:

testAccountList.cmp

```
  ...
  <aura:attribute name="resultSize" type="Integer" default="5">
  ...
```

testAccountListController.js

```
doInit : function(component) {
  var action = component.get("c.getAccounts");
  action.setParams({
    resultSize : component.get("v.resultSize")
  });
  action.setCallback(this, function(response) {
    ...
  });
  $A.enqueueAction(action);
}
```

TestAccountListController.cls

```
public class TestAccountListController {

  @AuraEnabled
  public static List&lt;Account&gt; getAccounts(Integer resultSize) {
    query = 'SELECT Id, Name FROM Account';
    if (resultSize != null && resultSize > 0) {
      query += ' LIMIT ' + resultSize;
    }
    return Database.query(query);
  }
}
```

A pretty straightward example of a Lightning Component calling a [server-side action](https://developer.salesforce.com/docs/atlas.en-us.lightning.meta/lightning/controllers_server_actions_call.htm). Or so I thought.

It turns out, running this in Lightning Experience throws a **Script-Thrown Exception**, and it is the kind that gives no context as to what the actual error is, even in the debug log. I quickly noticed this only occurred when going through the full end-to-end flow; unit tests worked perfectly. After a decent amount of debugging by dumping different test values to the debug log using System.debug, I realized that the error was being thrown during the **resultSize > 0** comparison, and more specifically the error that finally showed up implied that my resultSize Integer was acting as...a String?

Searching on the internet, I came across a [StackExchange post](http://salesforce.stackexchange.com/questions/108355/limit-expression-must-be-of-type-integer-error-when-using-apex-variable-in-soq/108423#108423) with confirmation that this is a confirmed bug (unfortunately it looks like it was filed a year ago). Since I needed to complete the work, I had to cast my Integer to an Integer to get the code to execute correctly (and added a comment to absolve my future self of ridicule).

TestAccountListController.cls

```
public class TestAccountListController {

  @AuraEnabled
  public static List<Account> getAccounts(Integer resultSize) {
    // Salesforce bug with Lightning serializer requires re-casting this value
    resultSize = Integer.valueOf(resultSize);

    query = 'SELECT Id, Name FROM Account';
    if (resultSize != null && resultSize > 0) {
      query += ' LIMIT ' + resultSize;
    }
    return Database.query(query);
  }
}
```
