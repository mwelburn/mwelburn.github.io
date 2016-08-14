---
id: 356
title: Working with SharePoint 2010 Web Services
date: 2013-04-04T07:00:20+00:00
author: Michael Welburn
layout: post
guid: http://michaelwelburn.com/?p=356
permalink: /2013/04/04/working-with-sharepoint-2010-web-services/
categories:
  - API
  - Programming
  - Salesforce
tags:
  - Apex
  - API
  - sharepoint
  - soapaction
  - WSDL
---
Recently I have been leveraging SharePoint 2010&#8217;s <a title="SharePoint 2010 Web Services" href="http://msdn.microsoft.com/en-us/library/ee705814(v=office.14).aspx" target="_blank">Web Services</a> from an external application (not on the Microsoft stack), and ran into a couple items that didn&#8217;t seem very well documented for a developer who wasn&#8217;t already familiar with the inner details how SharePoint worked (e.g. myself).

<!--more-->

The first roadblock that I ran into was trying to figure out how to authenticate with SharePoint and then make subsequent calls with some sort of authentication token. I learned that SharePoint&#8217;s login mechanism is setting a Cookie on the response header that contains the authentication token, unlike other APIs that I have worked with. Originally this baffled me a bit, as the HTTP response body only has the name of the Cookie, and the documentation does not really make note that you need to look in the response headers for the Cookie.

Unfortunately, this was coupled with leveraging Salesforce APEX classes via the SharePoint Authentication WSDL. When working with the APEX classes that Salesforce generates via the WSDL, there are two parameters on the class that you instantiate prior to invoking the method, inputHttpHeaders\_x and outputHttpHeaders\_x, that will not return any values _unless_ you instantiate them (they are maps of the request and response headers) prior to invoking the method. This was pretty frustrating, as I didn&#8217;t realize this until digging around through some random <a title="WSDL2Apex" href="http://www.salesforce.com/us/developer/docs/apexcode/Content/apex_callouts_wsdl2apex.htm" target="_blank">documentation</a> led me to it (and seems strangely unintuitive to purposely not set values, but I&#8217;m sure there is some good reason for it). This led to a lot of confusion as to where the Cookie was on the response until I discovered this tidbit.

After resolving that, I ran into another problem later when testing out the simplest API call I could find, GetLists.

> Apex type not found for _element List_

Essentially what I discovered is that Salesforce&#8217;s parsing of the response is extremely picky, and the classes it generates off the WSDL aren&#8217;t always an exact match for the response coming back. If there is any difference in the SOAP XML it will throw this error. What we ended up doing was simply identifying the SOAP envelope and constructing and destructing it ourselves in lieu of trying to debug the generated classes and cryptic errors.

The last issue I had was trying to <a title="CopyIntoItems" href="https://portal.sonomapartners.com/sonomasfdc/_vti_bin/Copy.asmx?op=CopyIntoItems" target="_blank">upload a file</a> via the API, as I kept running into a security related error.

> The security validation for this page is invalid. Click Back in your Web browser, refresh the page, and try your operation again

After doing more research than I thought was necessary, I finally happened upon a <a title="Invalid Quote" href="http://weblogs.asp.net/jan/archive/2009/05/25/quot-the-security-validation-for-this-page-is-invalid-quot-when-calling-the-sharepoint-web-services.aspx" target="_blank">post</a> that detailed what my issue was. I needed to set a SOAPAction header to the endpoint of the schema&#8217;s method I was invoking, in this case http://schemas.microsoft.com/sharepoint/soap/CopyIntoItems. In hindsight, taking a look at the example SOAP request for this method via http://<Site>/\_vti\_bin/Copy.asmx?op=CopyIntoItems would have saved me some time, as it does show this header there.

<img class="size-full wp-image-357 aligncenter" alt="Screen Shot 2013-04-03 at 8.51.33 PM" src="http://michaelwelburn.com/wp-content/uploads/2013/04/Screen-Shot-2013-04-03-at-8.51.33-PM.png" width="639" height="620" srcset="http://michaelwelburn.com/wp-content/uploads/2013/04/Screen-Shot-2013-04-03-at-8.51.33-PM.png 639w, http://michaelwelburn.com/wp-content/uploads/2013/04/Screen-Shot-2013-04-03-at-8.51.33-PM-300x291.png 300w" sizes="(max-width: 639px) 100vw, 639px" />