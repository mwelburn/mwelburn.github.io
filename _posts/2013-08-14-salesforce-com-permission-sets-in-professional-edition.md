---
id: 450
title: Salesforce.com Permission Sets in Professional Edition
date: 2013-08-14T09:00:55+00:00
author: Michael Welburn
layout: post
guid: http://michaelwelburn.com/?p=450
permalink: /2013/08/14/salesforce-com-permission-sets-in-professional-edition/
categories:
  - Salesforce
tags:
  - Permission Sets
  - Professional Edition
  - Salesforce
---
One of the major limitations of Salesforce.com Professional Edition is that there is no ability to customize Profiles, meaning that all of your users are stuck using one of the <a title="Standard Profiles" href="http://login.salesforce.com/help/doc/en/standard_profiles.htm" target="_blank">standard profiles</a>. While it is possible that your organization is small enough that you organization can be managed with standard profiles, particularly if you are extremely standard in your implementation of Salesforce.com, bigger organizations will quickly realize that the standard profiles are restricting their ability to mirror their users real life responsibilities. If upgrading to Enterprise Edition licenses or adding on the cost of upgrading Professional Edition to get Profiles are not options, there is one out of the box alternative to take a look at: <a title="Permission Sets" href="http://login.salesforce.com/help/doc/en/perm_sets_overview.htm" target="_blank">Permission Sets</a>.

<!--more-->

While Profiles are what most people are aware of for general user access of object types, responsibilities, and much more in a Salesforce.com organization, some people may not be familiar with Permission Sets. Essentially what they are is a way to extend a Profile (e.g. give it slightly more access) without having to create a duplicate Profile and modify it. This really starts paying dividends when a large batch of users all start with the same access, but a few users get access to Object A, a few others get access to Object B, etc. It allows you to maintain the base profile (and make any changes to it later in one profile instead of multiple), but quickly give users additional permissions.

While this sounds great, upon first glance in your Professional Edition organization you&#8217;ll notice that Permission Sets are available to use but none exist out of the box, and there is no option to create them. However, since they are accessible, that gives us a chance to try and find some workarounds. The first thing I thought about was whether managed packages from the AppExchange could provide permission sets for organizations to use, and spawning from that thought was a hypothesis to test: could we create our own Permission Sets in a Developer Edition organization and import them into a Professional Edition organization?

Interestingly, the answer is yes. You can actually set up a Developer Edition organization at <a title="http://developer.force.com" href="http://developer.force.com" target="_blank">http://developer.force.com</a>, create a permission set based on the security enhancements you require (say, for standard users who need just a bit more power), add it to an unmanaged package, generate the link, and then use it to install in your Professional Edition organization.

### Steps to Implement

<li style="text-align: left;">
  Sign up for a Developer Edition account at <a title="http://developer.force.com" href="http://developer.force.com" target="_blank">http://developer.force.com</a>
</li>
  1. Login to the Developer Edition organization and create a Permission Set (or multiple!) under Setup -> Administer -> Permission Sets, setting whatever values you deem necessary. 
    <li style="text-align: left;">
      <strong>Note: </strong>If you want to leverage custom objects/fields, ensure that you create them on the Developer Edition organization before the Professional Edition organization. This is because the Package you create will determine dependencies, and will notice that the Permission Set needs that custom object/field to be part of the Package. Then, when you go to install it in your Professional Edition organization, you don&#8217;t want the object/field to already exist (you may have to delete your existing field).
    </li>
    
    [<img class="aligncenter size-full wp-image-463" alt="Screen Shot 2013-08-07 at 10.10.41 PM" src="http://michaelwelburn.com/wp-content/uploads/2013/08/Screen-Shot-2013-08-07-at-10.10.41-PM.png" width="694" height="470" srcset="http://michaelwelburn.com/wp-content/uploads/2013/08/Screen-Shot-2013-08-07-at-10.10.41-PM.png 694w, http://michaelwelburn.com/wp-content/uploads/2013/08/Screen-Shot-2013-08-07-at-10.10.41-PM-300x203.png 300w" sizes="(max-width: 694px) 100vw, 694px" />](http://michaelwelburn.com/wp-content/uploads/2013/08/Screen-Shot-2013-08-07-at-10.10.41-PM.png)</li> 
    
      * Navigate to Setup -> Build -> Create -> Packages.
  
        [<img class="aligncenter size-full wp-image-462" alt="Screen Shot 2013-08-07 at 10.11.14 PM" src="http://michaelwelburn.com/wp-content/uploads/2013/08/Screen-Shot-2013-08-07-at-10.11.14-PM.png" width="678" height="467" srcset="http://michaelwelburn.com/wp-content/uploads/2013/08/Screen-Shot-2013-08-07-at-10.11.14-PM.png 678w, http://michaelwelburn.com/wp-content/uploads/2013/08/Screen-Shot-2013-08-07-at-10.11.14-PM-300x206.png 300w" sizes="(max-width: 678px) 100vw, 678px" />](http://michaelwelburn.com/wp-content/uploads/2013/08/Screen-Shot-2013-08-07-at-10.11.14-PM.png)
      * Create a new Package.
  
        [<img class="aligncenter size-full wp-image-461" alt="Screen Shot 2013-08-07 at 10.11.29 PM" src="http://michaelwelburn.com/wp-content/uploads/2013/08/Screen-Shot-2013-08-07-at-10.11.29-PM.png" width="677" height="389" srcset="http://michaelwelburn.com/wp-content/uploads/2013/08/Screen-Shot-2013-08-07-at-10.11.29-PM.png 677w, http://michaelwelburn.com/wp-content/uploads/2013/08/Screen-Shot-2013-08-07-at-10.11.29-PM-300x172.png 300w" sizes="(max-width: 677px) 100vw, 677px" />](http://michaelwelburn.com/wp-content/uploads/2013/08/Screen-Shot-2013-08-07-at-10.11.29-PM.png)
      * Add your Permission Set(s) to the package.
  
        [<img class="aligncenter size-full wp-image-460" alt="Screen Shot 2013-08-07 at 10.11.48 PM" src="http://michaelwelburn.com/wp-content/uploads/2013/08/Screen-Shot-2013-08-07-at-10.11.48-PM.png" width="689" height="342" srcset="http://michaelwelburn.com/wp-content/uploads/2013/08/Screen-Shot-2013-08-07-at-10.11.48-PM.png 689w, http://michaelwelburn.com/wp-content/uploads/2013/08/Screen-Shot-2013-08-07-at-10.11.48-PM-300x148.png 300w" sizes="(max-width: 689px) 100vw, 689px" />](http://michaelwelburn.com/wp-content/uploads/2013/08/Screen-Shot-2013-08-07-at-10.11.48-PM.png)
      * Click Upload.
  
        [<img class="aligncenter size-full wp-image-459" alt="Screen Shot 2013-08-07 at 10.11.56 PM" src="http://michaelwelburn.com/wp-content/uploads/2013/08/Screen-Shot-2013-08-07-at-10.11.56-PM.png" width="677" height="366" srcset="http://michaelwelburn.com/wp-content/uploads/2013/08/Screen-Shot-2013-08-07-at-10.11.56-PM.png 677w, http://michaelwelburn.com/wp-content/uploads/2013/08/Screen-Shot-2013-08-07-at-10.11.56-PM-300x162.png 300w" sizes="(max-width: 677px) 100vw, 677px" />](http://michaelwelburn.com/wp-content/uploads/2013/08/Screen-Shot-2013-08-07-at-10.11.56-PM.png)
      * Give it a version name. 
          * **Note: **There are many options below. For something as simple as this you won&#8217;t need to select anything else (particularly because most of the options won&#8217;t apply to Professional Edition).
        
        [<img class="aligncenter size-full wp-image-458" alt="Screen Shot 2013-08-07 at 10.12.12 PM" src="http://michaelwelburn.com/wp-content/uploads/2013/08/Screen-Shot-2013-08-07-at-10.12.12-PM.png" width="681" height="298" srcset="http://michaelwelburn.com/wp-content/uploads/2013/08/Screen-Shot-2013-08-07-at-10.12.12-PM.png 681w, http://michaelwelburn.com/wp-content/uploads/2013/08/Screen-Shot-2013-08-07-at-10.12.12-PM-300x131.png 300w" sizes="(max-width: 681px) 100vw, 681px" />](http://michaelwelburn.com/wp-content/uploads/2013/08/Screen-Shot-2013-08-07-at-10.12.12-PM.png)</li> 
        
          * Congratulations! You have created an unmanaged package. Now that you have the link, you can copy it and log out of your Developer Edition organization. Then paste the link into your browser, authenticate with your Professional Edition credentials, and start applying your Permission Set(s) to users!
  
            [<img class="aligncenter size-full wp-image-457" alt="Screen Shot 2013-08-07 at 10.13.17 PM" src="http://michaelwelburn.com/wp-content/uploads/2013/08/Screen-Shot-2013-08-07-at-10.13.17-PM.png" width="675" height="416" srcset="http://michaelwelburn.com/wp-content/uploads/2013/08/Screen-Shot-2013-08-07-at-10.13.17-PM.png 675w, http://michaelwelburn.com/wp-content/uploads/2013/08/Screen-Shot-2013-08-07-at-10.13.17-PM-300x184.png 300w" sizes="(max-width: 675px) 100vw, 675px" />](http://michaelwelburn.com/wp-content/uploads/2013/08/Screen-Shot-2013-08-07-at-10.13.17-PM.png)</ol>