---
id: 425
title: 'Salesforce.com: Introduction to the Ant Migration Tool'
date: 2013-08-21T09:00:49+00:00
author: Michael Welburn
layout: post
guid: http://michaelwelburn.com/?p=425
permalink: /2013/08/21/salesforce-com-introduction-to-the-ant-migration-tool/
categories:
  - Programming
  - Salesforce
tags:
  - Ant
  - Metadata API
  - Migration Tool
  - Salesforce
  - XML
---
Working with Salesforce.com for the last few years, I&#8217;ve been lucky that I haven&#8217;t had to work with migrating changes between organizations often. For the most part I&#8217;ve dealt with production and sandbox organization changes, and have mostly used <a title="Change Sets" href="http://help.salesforce.com/help/doc/en/changesets.htm" target="_blank">Change Sets</a> to propagate those. The only time I had to migrate code between organizations, I leveraged the Eclipse-based <a title="Force.com IDE" href="http://wiki.developerforce.com/page/Force.com_IDE" target="_blank">Force.com IDE</a> to extract and deploy all the changes for a complete organization copy. Needless to say, this was an **extremely** time intensive process, for reasons that I will outline below. Recently, I knew I&#8217;d have another migration, so I spent some time discovering what the best practice for doing the same would be today in order to prepare myself.

<!--more-->

While in the past this might have been accomplished with an unmanaged package, I quickly discovered that the go-to tool for this sort of process was the <a title="Migration Tool Guide" href="http://wiki.developerforce.com/page/Migration_Tool_Guide" target="_blank">Salesforce.com Migration Tool</a>, which is invoked as an Ant based command line tool that leverages the Salesforce.com <a title="Metadata API" href="http://www.salesforce.com/us/developer/docs/dev_lifecycle/Content/plan_proj_metadata.htm" target="_blank">Metadata API</a>. To be a bit more specific, the Migration Tool allows you to define a custom list of metadata components via an XML that you can either pull down or push up to a Salesforce.com organization to/from your local computer. This package.xml file can be thought of as an XML based version of what you might set up in a Change Set between a sandbox and production organization, but the Migration Tool as a whole is a bit more powerful.

One of the best parts about using the Migration Tool instead of the Force.com IDE is the lack of user interaction required, particularly for a deploy. In the Force.com IDE, each deploy requires a user go through a few different pages in an on screen wizard to type in their credentials, verify what metadata to migrate, etc. In a vacuum, this only takes a minute or two, but when running the same migration constantly (for instance, resolving errors regarding dependent metadata &#8212; e.g. pulling over a formula field but forgetting to include a custom field it references), it can get aggravating. This particular issue (resolving dependencies while trying to install an entire organization to another organization) was responsible for most of the time consumed the first time that I tried using the Force.com IDE to accomplish the task. The ability to run a single command line repeatedly really makes life much easier. The fact the the Migration Tool executes both pushes and pulldowns significantly faster than the Force.com IDE doesn&#8217;t hurt either!

For anyone that has experience with the Force.com IDE, you&#8217;ll notice that the file structure is exactly the same as when you pull down your Apex classes and Visualforce Pages for development. Both the Force.com IDE and the Migration Tool leverage the package.xml to define what parts of the Salesforce.com organization need to be pulled down locally, as well as what to push back up to Salesforce.com when necessary.

The other great thing about getting comfortable with the Migration Tool is that by pulling down your changes, you&#8217;ll always have a great idea of what was modified between organizations, because you can store the individual migrations in your version control system. This is actually the preferred way of handling multiple developer projects in order for each person to develop in their own sandbox, pull other developers changes in locally, and push from the version control system to production. One thing to note about this, however, is that when working with a production organization with AppExchange packages that custom code is dependent on, ensure that the package can be installed to a developer edition of Salesforce.com. If not, you will often end up needing to share a sandbox with the team (unless the production org is Unlimited Edition and has Developer sandboxes to spare, or the owner of the organization is willing to pay for additional sandboxes).

### Tips

I did come across a few things that jumped out at me as things to keep note of.

  * Use the Force.com IDE to jump start your package.xml. Because they use the same format, the initial creation of the XML file is much more easily done via the Add/Remove Metadata Properties within the IDE because it allows you to see a visual representation of the organization and selectively choose them. This is obviously much more efficient than manually typing in your components (when possible).
  * If you want to bring over components that you customized, but were originally installed via a managed package (e.g. you added some custom fields to an installed object, or you customized a picklist on this object), you won&#8217;t be able to view those in the Force.com IDE GUI. These you will have to manually type into the package.xml. The format for these are pretty easy to glean from other fields, but for a custom field it would look like:

> <div>
>   <types>
> </div>
> 
> <div style="padding-left: 30px;">
>   <members>MW__CustomObject__c.MyField__c</members>
> </div>
> 
> <div style="padding-left: 30px;">
>   <name>CustomField</name>
> </div>
> 
> <div>
>   </types>
> </div>

  * Be careful when adding things manually, particularly components that have non-alphanumeric characters. Be sure to HTML escape when necessary. These include slashes (%2F), commas (%2E), parenthesis (%28 and %29), etc.
  * There are certain metadata components that you won&#8217;t be able to extract. Normally these are pieces that rely on individual users, such as approval processes. Be aware that you might have to manually recreate these in the other organization.
  * If you attempt to update Apex classes that already exist in the target organization and are scheduled to run as Apex Scheduled Jobs, you will have to delete the jobs, upload the Apex class changes, and re-schedule the jobs.
  * If you are looking to ever _delete_ components from an organization, the package.xml file is not going to help you. You&#8217;ll need to create a destructiveChanges.xml file and add the components to delete there.
  * If History Tracking is turned on for a standard field in your source organization and is disabled in the target for an object you are migrating over, the Metadata API will throw an error. I had to manually turn on History Tracking in this case when I was trying to pull over all updates of the Account object.
  * I got the following error when migrating Quotes to a new organization. I had forgotten that you have to <a title="Enable Quotes" href="https://help.salesforce.com/HTViewHelpDoc?id=quotes_enable.htm&language=en_US" target="_blank">enable Quotes</a> in an organization, so keep in mind that you may have to manually enable some features.

> Error: objects/Quote.object(Quote):Invalid fullName, must end in __c
  
> Error: objects/QuoteLineItem.object(QuoteLineItem):Invalid fullName, must end in __c

  * I did notice more issues coming up when I tried to export and import entire standard objects rather than pieces (e.g. identifying a handful of fields I customized). I got a variety of errors, the most confusing below (which I haven&#8217;t figured out yet):

> Error: objects/Account.object(Account):duplicate value found: <unknown> duplicates value on record with id: <unknown>

I ended up leveraging the Migration Tool for a few smaller tasks in lieu of Change Sets just to try it out, and really started to enjoy using it (although I had some previous experience with Ant). As a developer, the fact that I can keep track of changes in version control rather than auditing Change Sets in Salesforce.com is a much safer practice, although both have a reliance of ensuring that there is some sort of procedure in place to limit editing of configurations in production if you are also looking to push changes in from a sandbox.

### Future Potential

Playing with the Migration Tool opened my eyes to a few different things that I am looking to explore now, and some possible new ideas that would make its use a bit easier.

  * The main issue that I see with the Migration Tool in its current state is that it is very much geared towards developers. I can&#8217;t imagine someone who isn&#8217;t comfortable with the command line or XML being able to do anything except run it after the environment and package.xml are set up. An interesting possibility around this is creating a GUI to pull and push metadata leveraging the Metadata API, something like the Force.com IDE&#8217;s Add/Remove Metadata Properties page extracted into a simpler application.
  * One of the nice pieces about creating an Ant script for deploying changes is that you can automate it! There have been a few different blogs about leveraging the Migration Tool with continuous integration (<a title="Cruise Control" href="http://wiki.developerforce.com/page/Continuous_Integration_Cruise_Control_and_Force_Com" target="_blank">here</a> and <a title="Jenkins" href="http://blogs.developerforce.com/developer-relations/2013/03/setting-up-jenkins-for-force-com-continuous-integration.html" target="_blank">here</a>) to deploy changes to production (or a QA environment for people to test without having to deal with accessing developer environments or requiring manual setup of new/updated environments).

I&#8217;m interested to hear any other experiences people have had with the tool, particularly ways that I haven&#8217;t thought about that will help me be more efficient working with Salesforce.com.