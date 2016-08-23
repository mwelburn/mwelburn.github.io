---
id: 626
title: Removing Labels from Visualforce Charting Pie Wedges
date: 2014-01-06T06:00:44+00:00
author: Michael Welburn
guid: http://michaelwelburn.com/?p=626
permalink: /2014/01/06/removing-labels-from-visualforce-charting-pie-wedges/
categories:
  - Javascript
  - Salesforce
tags:
  - JavaScript
  - Salesforce
  - Visualforce
  - Visualforce Charting
---
While leveraging <a title="A Simple Charting Example" href="http://www.salesforce.com/us/developer/docs/pages/Content/pages_charting_overview_simple_example.htm" target="_blank">Visualforce charting</a>, I came across a few instances where I found that having the label of the wedges of a pie chart a bit off-putting. This is often the case when you have a legend or key right next to the pie chart because having the labels on top of the wedges is somewhat unreadable due to either the size of the wedges or the number of wedges.

[<img class="aligncenter size-medium wp-image-705" alt="pie-wedge-visualforce" src="http://michaelwelburn.com/wp-content/uploads/2013/12/pie-wedge-visualforce-300x168.png" width="300" height="168" srcset="http://michaelwelburn.com/wp-content/uploads/2013/12/pie-wedge-visualforce-300x168.png 300w, http://michaelwelburn.com/wp-content/uploads/2013/12/pie-wedge-visualforce.png 312w" sizes="(max-width: 300px) 100vw, 300px" />](http://michaelwelburn.com/wp-content/uploads/2013/12/pie-wedge-visualforce.png)

<!--more-->

I originally thought I could modify my data source, which consisted of a List of PieWedgeData records, to just make the labels equal to an empty string. Unfortunately, setting 3 wedges to empty strings had some unintended consequences around the rest of my usage of that data source.

You also might think that there is an option in the <a title="apex:pieSeries" href="http://www.salesforce.com/us/developer/docs/pages/Content/pages_compref_pieSeries.htm" target="_blank"><apex:pieSeries></a> tag. Unfortunately, tips was the closest thing I could find, and that was just for tooltips on hover.

Instead, I decided to put on my developer hat and turn on the <a title="Chrome Developer Tools" href="https://developers.google.com/chrome-developer-tools/" target="_blank">Chrome Developer Tools</a>. It isn&#8217;t every day that I get a chance to hunt down something that (as far as I can tell) is undocumented, so I took advantage of it.

After deciding that the rendererFn attribute on the <apex:pieSeries> was my best bet, I set on a path of following the javascript implementation by Salesforce to see what the underlying javascript objects looked like by putting a breakpoint inside the javascript method I created to set that attribute to.

[<img class="aligncenter size-full wp-image-708" alt="debug-javascript" src="http://michaelwelburn.com/wp-content/uploads/2013/12/debug-javascript.png" width="513" height="70" srcset="http://michaelwelburn.com/wp-content/uploads/2013/12/debug-javascript.png 513w, http://michaelwelburn.com/wp-content/uploads/2013/12/debug-javascript-300x40.png 300w" sizes="(max-width: 513px) 100vw, 513px" />](http://michaelwelburn.com/wp-content/uploads/2013/12/debug-javascript.png)

[<img class="aligncenter size-full wp-image-709" alt="chrome-dev-tools" src="http://michaelwelburn.com/wp-content/uploads/2013/12/chrome-dev-tools.png" width="319" height="568" srcset="http://michaelwelburn.com/wp-content/uploads/2013/12/chrome-dev-tools.png 319w, http://michaelwelburn.com/wp-content/uploads/2013/12/chrome-dev-tools-168x300.png 168w" sizes="(max-width: 319px) 100vw, 319px" />](http://michaelwelburn.com/wp-content/uploads/2013/12/chrome-dev-tools.png)

What I found was that if I set a value called the label.rendered to an empty string, it would in turn take my labels on that pieChart and ignore them.

    <script type="text/javascript">
    function pieRenderer(klass, item, e, f, g, h)
    {
      //overwrite the label rendering to be blank
      this.label.renderer = new function() { return ''; };
    }
    </script>

    <apex:chart animate="true" height="200" width="200" colorSet="#E93,#6AC,#AD2" data="{!myData}">
      <apex:pieSeries tips="true" highlight="false" dataField="data" <strong>rendererFn="pieRenderer"</strong>/>
    </apex:chart>

The only thing that I noticed as a side effect was that I also lost that drop shadow on the pie chart overall. As that wasn&#8217;t an issue for me, I haven&#8217;t spent any additional time trying to figure that out.
