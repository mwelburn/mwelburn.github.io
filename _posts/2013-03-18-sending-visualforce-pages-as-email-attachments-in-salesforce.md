---
id: 229
title: Sending Visualforce Pages as Email Attachments in Salesforce
date: 2013-03-18T07:00:59+00:00
author: Michael Welburn
guid: http://michaelwelburn.com/?p=229
permalink: /2013/03/18/sending-visualforce-pages-as-email-attachments-in-salesforce/
categories:
  - Programming
  - Salesforce
tags:
  - email
  - pdf
  - quote
  - Salesforce
  - Visualforce
---
Recently I had to create a very complex Quoting mechanism in Salesforce that included a lot of custom product dependencies and limitations. In order to accomplish this, I created a custom Visualforce page that ended up having a lot of business logic, and leveraged the Quote Line Item objects as generically as it could. However, the client preferred to have their quote look exactly like their existing Excel generated quote, which was not possible out of the box with Salesforce.

In order to accomplish this, I created a second Visualforce page that was <a title="PDF Documents with Visualforce" href="http://wiki.developerforce.com/page/Creating_Professional_PDF_Documents_with_CSS_and_Visualforce" target="_blank">rendered</a> as a PDF, which was accessed by a custom button on the Quote page layout. Unfortunately, using a custom Visualforce page instead of the out of the box quotes removed some of the functionality around quoting, in particular the ability to Save Quotes as PDF, and the ability to send PDF Quotes out to customers.

<!--more-->

Fortunately, there is a pretty easy way to do both. By wrapping the custom Visualforce Quote inside another Visualforce page (both using the same custom APEX controller where the parameter was the Quote ID), I was able to add a few custom buttons to give the user the same functionality.

<img class="alignnone size-full wp-image-293 aligncenter" alt="Screen Shot 2013-03-03 at 8.06.01 PM" src="http://michaelwelburn.com/wp-content/uploads/2013/03/Screen-Shot-2013-03-03-at-8.06.01-PM.png" width="578" height="110" srcset="http://michaelwelburn.com/wp-content/uploads/2013/03/Screen-Shot-2013-03-03-at-8.06.01-PM.png 578w, http://michaelwelburn.com/wp-content/uploads/2013/03/Screen-Shot-2013-03-03-at-8.06.01-PM-300x57.png 300w" sizes="(max-width: 578px) 100vw, 578px" />

In order to save a Quote as a PDF, provided you have the Quote ID, you simply have to leverage the following code (which in this case is wired up to the &#8220;Save to Quote&#8221; button.

    // Assuming quote is a Quote variable set in the controller

    public PageReference saveQuoteAsPDF()
    {
       PageReference pr = Page.VisualforceQuotePDF;
       pr.getParameters().put('id', quote.id);
       Blob content = pr.getContentAsPDF();
       QuoteDocument doc = new QuoteDocument(Document = content, QuoteId = quote.id);

       Database.SaveResult insertResult = Database.Insert(doc, false);
       // check for errors here and act according!
       System.debug('Result: ' + insertResult);
       System.debug('Quote PDF created: ' + doc.Id);

       //upon completion redirect to quote
       PageReference quotePage = new PageReference('/' + quote.id);
       quotePage.setRedirect(true);
       return quotePage;
    }

Pretty impressively, the above code will actually treat the Quote as a new version and name it accordingly.

There are some good posts around sending emails with APEX, as seen <a title="Create and Email a PDF with Visualforce" href="http://blog.jeffdouglas.com/2010/07/16/create-and-email-a-pdf-with-salesforce-com/" target="_blank">here</a> and <a title="Sending Email Attachments" href="http://www.salesforce.com/us/developer/docs/pages/Content/pages_email_sending_attachments.htm" target="_blank">here</a>. I had a slightly different need, as I was not automating an email, but instead wanted to save the Quote and then allow the user to leverage Salesforce&#8217;s email page to create a personalized message with the Quote PDF attached.

Completing this task is pretty simple as well, once you get used to interpreting Salesforce.com URLs. By essentially replicating this process via the UI (clicking the default Save & Email Quote button) you can see what the URL parameters look like. Then, in your APEX code, you can replicate the same URL pattern. While there is always the caveat of Salesforce changing their URL param structure in the future, I haven&#8217;t had any issues with it in the past, and there are plenty of other snippets of code out there that leverage URL parameter customization (particularly if you want to navigate to a Page Layout and <a title="Prepopulate Fields" href="http://raydehler.com/cloud/clod/salesforce-url-hacking-to-prepopulate-fields-on-a-standard-page-layout.html" target="_blank">populate</a> some fields based on where you came from).

    // Assuming quote is a Quote variable set in the controller
    // Assuming quoteContact is the contact on the Quote object

    public PageReference saveQuoteAsPDFandEmail()
    {       
       PageReference pr = Page.VisualforceQuotePDF;
       pr.getParameters().put('id', quote.id);
       Blob content = pr.getContentAsPDF();
       QuoteDocument doc = new QuoteDocument(Document = content, QuoteId = quote.id);

       Database.SaveResult insertResult = Database.Insert(doc, false);
       // check for errors here and act according!
       System.debug('Result: ' + insertResult);
       System.debug('Quote PDF created: ' + doc.Id);  

       //upon completion redirect to quote
       String emailURL = '/_ui/core/email/author/EmailAuthor?p3_lkid=' + quote.Id + '&doc_id=' + doc.Id + '&retURL=%2F' + quote.Id;
       if (quoteContact != null)
       {
          emailURL += '&p2_lkid=' + quoteContact.Id;
       }
       System.debug('Quote Email URL: ' + emailURL);
       PageReference quotePage = new PageReference(emailURL);
       quotePage.setRedirect(true);
       return quotePage;       
    }

In this example, we also leverage the Contact on the Quote to set an initial &#8220;To&#8221; address. You can also see the quote ID and doc ID (the new Quote PDF we saved) in the URL params. You have the ability to add even more fields, such as the subject, if you want.

An added benefit of this logic is that it allows you to attach different business logic to your users&#8217; ability to create Quote PDFs. For instance, if they are discounting products too heavily, you can simply not render the save buttons, or even the Quote PDF itself, and instead relay a message to the user that they need to submit the quote for approval first.
