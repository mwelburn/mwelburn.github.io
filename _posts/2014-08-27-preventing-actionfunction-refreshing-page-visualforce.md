---
id: 987
title: Preventing a Visualforce ActionFunction from Refreshing the Page
date: 2014-08-27T22:55:59+00:00
author: Michael Welburn
layout: post
guid: http://michaelwelburn.com/?p=987
permalink: /2014/08/27/preventing-actionfunction-refreshing-page-visualforce/
twitterCardType:
  - summary
cardImgSize:
  - small
cardImageWidth:
  - 280
cardImageHeight:
  - 150
categories:
  - Programming
  - Salesforce
tags:
  - Visualforce
---
I came across a restriction the other day regarding the <a title="apex:actionFunction" href="https://www.salesforce.com/us/developer/docs/pages/Content/pages_compref_actionFunction.htm" target="_blank">apex:actionFunction</a> Visualforce component when I was trying to implement a to-do list that allowed a user to complete a to-do with a click on a checkbox next to a list of to-dos. While using one of the many great two way binding Javascript frameworks like <a title="AngularJs" href="https://angularjs.org/" target="_blank">AngularJS</a> with a <a title="RemoteAction" href="https://www.salesforce.com/us/developer/docs/apexcode/Content/apex_classes_annotation_RemoteAction.htm" target="_blank">@RemoteAction</a> annotated Apex method would have worked great, it was a bit of overkill for this, as using a <a title="StandardSetController" href="https://www.salesforce.com/us/developer/docs/pages/Content/apex_pages_standardsetcontroller.htm" target="_blank">StandardSetController</a> required minimal code.

Functionally, that all worked fine. The problem I ran into when I was setting up my code was that my entire Visualforce page was refreshing when I invoked the actionFunction. Even though I tried both setting the invoked Apex method to a return type of void (and later tried returning a null <a title="PageReference" href="https://www.salesforce.com/us/developer/docs/apexcode/Content/apex_system_pagereference.htm" target="_blank">PageReference</a>), the entire page refresh continued. Unfortunately this took away from the &#8220;behind the scenes save&#8221; behavior that I wanted the user to experience.

<!--more-->

What I ended up doing to resolve this was setting a **reRender** attribute on the actionFunction that pointed to an ID that didn&#8217;t even exist on my page. While I didn&#8217;t have anything that I actually wanted to re-render, it appears that setting that attribute stopped the default action of a full page refresh and instead traded for the _possibility_ of re-rendering an individual component instead.

The Visualforce page ended up looking like this very simple snippet of code:

    <apex:page controller="TodosController">
    
      ...
    
      <apex:form>
    
        <apex:repeat value="{!todos}" var="todo">
          <apex:inputField value="{!todo.Completed__c}" onchange="saveUpdates()"/>
          <apex:outputField value="{!todo.Name}"/>
        </apex:repeat>
    
        <!-- need to point to nonexistent ID so page doesn't refresh -->
        <apex:actionFunction name="saveUpdates" action="{!saveUpdates}" rerender="fakeresults"/>
    
      </apex:form>
    
    </apex:page>

Leveraging the StandardSetController makes implementing that custom **saveUpdates** method in the custom Apex controller quite easy:

    public class TodosController {
    
      public ApexPages.StandardSetController stdSetController;
    
      ...
    
      public List<Todo__c> getTodos() {
        return this.stdSetController.getRecords();
      }
    
      public void saveUpdates() {
        this.stdSetController.save();
      }
    }