---
id: 499
title: Salesforce.com Metadata API Field Integrity Exception
date: 2013-09-24T09:00:47+00:00
author: Michael Welburn
guid: http://michaelwelburn.com/?p=499
permalink: /2013/09/24/salesforce-com-metadata-api-field-integrity-exception/
categories:
  - API
  - Programming
  - Salesforce
tags:
  - Error
  - Metadata API
  - Salesforce
---
The other day I was doing a coworker a favor by pushing up a bunch of changes from a Salesforce sandbox to production using the <a title="Salesforce.com: Introduction to the Ant Migration Tool" href="http://michaelwelburn.com/2013/08/21/salesforce-com-introduction-to-the-ant-migration-tool/" target="_blank">Migration Tool</a>. I set up my package.xml file to pull down the pieces I needed from the sandbox and moved those customizations over to my production folder to push them up, but ran into the following error.

> Custom Field Definition &#8211; field integrity exception: unknown (required must not be specified)

<!--more-->



I didn&#8217;t have too many things in my XML, so I started pulling pieces of my migration out and re-testing, I finally narrowed the issue down to a particular picklist field.

The actual problem ended up being that the field was a required text field originally in production, but after a sandbox refresh it apparently was changed to be a picklist in the sandbox (which does not have the option of being required). Therefore, when I was trying to push the changes up it was making some effort to change the field type but could not recognize the required tag in the object XML when trying to save the definition. In this case, it was easiest for me to simply update the field to a picklist in production, and then run the Migration Tool again to update the picklist values and security from the sandbox.

For a project where you have more of these errors (to the point where manually changing the values is not viable), you could pull down the XML for the fields in question from production and set the required value to false for them (perhaps a macro) and save it, and then run your migration. Or at that point, you could just modify the field type in the XML (although the more untested changes you make in your production environment XML, the higher the chances of screwing things up).
