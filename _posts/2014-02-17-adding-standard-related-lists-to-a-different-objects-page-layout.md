---
id: 581
title: 'Adding Standard Related Lists to a Different Object&#8217;s Page Layout'
date: 2014-02-17T06:00:32+00:00
author: Michael Welburn
guid: http://michaelwelburn.com/?p=581
permalink: /2014/02/17/adding-standard-related-lists-to-a-different-objects-page-layout/
categories:
  - Programming
  - Salesforce
tags:
  - related list
  - Visualforce
---
Recently I was asked whether it was possible to show the Notes & Attachments related list from one record on another record&#8217;s page layout. The caveat was that the records were loosely related, but they were not of the same object type. That got me thinking about the use case beyond just Notes & Attachments to the idea that any related list could theoretically want to be shown on another record&#8217;s page layout (particularly when you want to highlight a parent&#8217;s related list on a child&#8217;s page layout).

<!--more-->

I came up with two possible solutions to this issue.

_(For the purpose of delivering a more readable solution, let&#8217;s pretend that we are trying to add the Notes & Attachments related list from the Account onto the related Contacts)_

### Add a Visualforce Page to the Page Layout that contains the table

In this scenario, which is what you are probably familiar with regarding how most AppExchange customizations work, there is a Visualforce Page that we drag onto the Page Layout. We would have to construct a Visualforce Page that referenced a StandardController of Contact (which is required in order to add the Visualforce Page to the Contact page layout). From there, there are two ways that we could handle this:

  * Build a controller extension, manually query for the Notes & Attachments, and build the table ourselves using <a title="http://www.salesforce.com/us/developer/docs/pages/Content/pages_compref_pageBlockTable.htm" href="http://www.salesforce.com/us/developer/docs/pages/Content/pages_compref_pageBlockTable.htm" target="_blank"><apex:pageBlockTable></a>.
  * Create _another_ Visualforce Page with a StandardController of Account and use an <a title="http://www.salesforce.com/us/developer/docs/pages/Content/pages_compref_iframe.htm" href="http://www.salesforce.com/us/developer/docs/pages/Content/pages_compref_iframe.htm" target="_blank"><apex:iframe url=&#8221;/apex/myPage?id=XYZ&#8221;></a> to reference that page, passing in the Account ID as a parameter. This would allow us to simply use the <a title="http://www.salesforce.com/us/developer/docs/pages/Content/pages_compref_relatedList.htm" href="http://www.salesforce.com/us/developer/docs/pages/Content/pages_compref_relatedList.htm" target="_blank"><apex:relatedList list=&#8221;NotesAndAttachments&#8221;/></a> tag to render it for us.

From a time perspective, the second is much more ideal, although you have to worry about getting the dimensions of your iframe correct based on what is going to be shown on the Page Layout (the last thing you want is scroll bars). However, the second option brings up another question, primarily whether you want to allow this Account related list to be **modified** from the Contact record. There is no way to remove the New / Edit / Delete buttons and links on the related list in this case besides restricting overall security.

In any case, I haven&#8217;t been a big fan of this solution for displaying custom &#8220;related lists&#8221;, primarily because I find it somewhat confusing when you have to put Visualforce Pages inside the detail section of the Page Layout (above the second set of buttons). In my mind, we would rather have a related list grouped together at the bottom, which brings me to the next option.

### Use a Visualforce Page to replace the standard Contact View layout

This solution has the same two implementation considerations as above, however instead of dragging our size constrained Visualforce page onto the Page Layout, we are going to create a Visualforce Page that renders the normal Page Layout, but then appends our related list on the end, using the <a title="http://www.salesforce.com/us/developer/docs/pages/Content/pages_compref_detail.htm" href="http://www.salesforce.com/us/developer/docs/pages/Content/pages_compref_detail.htm" target="_blank"><apex:detail></a> tag. After we create this Visualforce page, we can set it to be the default &#8220;View&#8221; action under _Customize -> Contacts -> Buttons, Links and Actions_.

[<img class="aligncenter size-full wp-image-715" alt="contact-view-override" src="http://michaelwelburn.com/wp-content/uploads/2013/12/contact-view-override.png" width="678" height="311" srcset="http://michaelwelburn.com/wp-content/uploads/2013/12/contact-view-override.png 678w, http://michaelwelburn.com/wp-content/uploads/2013/12/contact-view-override-300x137.png 300w, http://michaelwelburn.com/wp-content/uploads/2013/12/contact-view-override-668x306.png 668w" sizes="(max-width: 678px) 100vw, 678px" />](http://michaelwelburn.com/wp-content/uploads/2013/12/contact-view-override.png)

There are two benefits to this method:

  * Visualforce Pages dragged to a Page Layout **must** have a fixed height defined. For a related list, this isn&#8217;t as big of a deal as long as you are willing to display a set number of records and have a link to view more. You might get some vertical whitespace when you don&#8217;t have any records.
      * This solution can be as tall as you want.
  * The related list is going to have to go somewhere in the Page Layout&#8217;s field sections. The lowest you can make it is right after the bottom section of action buttons, right above the first related list.
      * This solution will go at the bottom of the Page.

For additional help, below is some sample source code for this solution:



### Results

I ended up going with the custom built table and replacing the standard Contact View with a Visualforce page. I felt that in this scenario, putting the related list at the bottom and having the flexibility of dynamic height was worthwhile.
