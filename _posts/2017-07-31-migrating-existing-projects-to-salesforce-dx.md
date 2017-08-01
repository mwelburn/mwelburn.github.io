---
title: Migrating Existing Projects to Salesforce DX
date: 2017-07-31T22:21:08+00:00
author: Michael Welburn
permalink: /2017/07/31/migrating-existing-projects-to-salesforce-dx/
twitterCardType:
  - summary
categories:
  - Salesforce
excerpt: "Identifying conflicts with package folders during a Salesforce DX migration."
---

I spent some time after [TrailheaDX](https://developer.salesforce.com/trailheadx) excited to try converting the existing Salesforce project I work on to the DX platform. There are a plethora of great online references on [Salesforce DX](https://developer.salesforce.com/platform/dx), the next generation of Salesforce developer experience that pushes it closer to parity with a typical development and deployment lifecycle. If you are looking to get started, [Christophe Coenraets](http://coenraets.org/) posted a [step by step instructional guide on migrating an existing project to Salesforce DX](https://developer.salesforce.com/blogs/developer-relations/2017/07/migrating-existing-projects-salesforce-dx.html) today on the Salesforce Developers Blog.

While I don't want to rehash the steps that Christophe nicely laid out, I did want to provide some assistance to anyone who ran into the same problem I had when trying to execute the steps. At the time I was working on setting up my project, I had been referencing the Salesforce DX Developer Guide Beta to understand the conversion process, outlined [here](https://developer.salesforce.com/docs/atlas.en-us.sfdx_dev.meta/sfdx_dev/sfdx_dev_ws_config.htm) and [here](https://developer.salesforce.com/docs/atlas.en-us.sfdx_dev.meta/sfdx_dev/sfdx_dev_ws_retrieve_pack_xml.htm). The issue in my case was that the ZIP file that got pulled down for the conversion process was created as *unpackaged.zip*, which I then unzipped to the project's root directory. Since the DX configuration example included /unpackaged as a package directory in the sfdx-project.json file, attempting to run the migration command kept returning a vague error.

    ERROR running force:mdapi:convert:  Unexpected file found in package directory: /Users/welburn/sfdx/unpackaged/package.xml.
    
In order to remedy this, I needed to either rename my folder of unzipped assets, or remove the reference to /unpackaged as a package directory in the configuration.

Overall, the process seemed much more streamlined that I anticipated (in theory). In practice, converting an existing production-level project to DX is quite an involved process in resolving dependencies, determining which platform features you have enabled in production are and are not supported in the DX configuration (and making manual changes in the interim). However, even testing a subset of the full functionality with scratch orgs left me very excited about a future in Salesforce development that minimizes the haphazard potential of collaboration and staying in sync with others.
