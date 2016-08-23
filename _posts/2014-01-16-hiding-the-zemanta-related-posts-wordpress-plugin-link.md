---
id: 746
title: Hiding the Zemanta Related Posts WordPress Plugin Link
date: 2014-01-16T09:00:06+00:00
author: Michael Welburn
guid: http://michaelwelburn.com/?p=746
permalink: /2014/01/16/hiding-the-zemanta-related-posts-wordpress-plugin-link/
categories:
  - Website
tags:
  - Wordpress
  - Zemanta
---
Back when I <a title="Migrating from Tumblr to WordPress" href="http://michaelwelburn.com/2013/02/18/migrating-from-tumblr-to-wordpress/" target="_blank">migrated my blog over to WordPress last year</a>, I had read the tips about how you should set up links to other pages on your site in order to limit user dropoff. In the spirit of that, I looked for a plugin on WordPress that would automatically set that up and maintain it for me. I came across <a title="Zemanta Related Posts" href="http://wordpress.org/plugins/related-posts-by-zemanta/" target="_blank">Zemanta Related Posts</a> and thought it was suitable for what I was looking to do. I&#8217;m still happy with the solution (and my 2% retention rate via related posts), but I found that I wasn&#8217;t a huge fan of the link that Zemanta put on my blog entries (see the bottom left of the image).

<img class="aligncenter size-full wp-image-752" alt="Zemanta Related Posts" src="http://michaelwelburn.com/wp-content/uploads/2014/01/Screen-Shot-2014-01-13-at-10.02.44-AM.png" width="512" height="228" srcset="http://michaelwelburn.com/wp-content/uploads/2014/01/Screen-Shot-2014-01-13-at-10.02.44-AM.png 512w, http://michaelwelburn.com/wp-content/uploads/2014/01/Screen-Shot-2014-01-13-at-10.02.44-AM-300x133.png 300w" sizes="(max-width: 512px) 100vw, 512px" />

<!--more-->

Back when I first noticed it, I figured there had to be some kind of setting to turn it off, but I soon realized that wasn&#8217;t the case. I ended up <a title="Plugin Editor Screen" href="https://codex.wordpress.org/Plugins_Editor_Screen" target="_blank">editing</a> the actual source code of the plugin to delete the link. You can see the culprit below under the &#8216;display\_zemanta\_linky&#8217; condition (the value of which deceptively isn&#8217;t set anywhere that you can modify).

[<img alt="Screen Shot 2014-01-13 at 9.58.21 AM" src="http://michaelwelburn.com/wp-content/uploads/2014/01/Screen-Shot-2014-01-13-at-9.58.21-AM.png" width="760" height="311" />](http://michaelwelburn.com/wp-content/uploads/2014/01/Screen-Shot-2014-01-13-at-9.58.21-AM.png)

While that worked, I started to realize every time Zemanta updated the plugin it would overwrite my changes with a new version of the file that would include the link again. As such, I had to remember to keep making this change.

After the most recent update, I realized that I could just hide the link with CSS. Fortunately the plugin has some pseudo name-spacing of the CSS classes that keeps the path pretty unique, so it shouldn&#8217;t affect anything else on the site. Instructions on how to get to the page where you edit your CSS are <a title="Editing WordPress CSS" href="http://en.support.wordpress.com/custom-design/editing-css/" target="_blank">here</a>.

    /* hide Zemanta link */
    div.wp_rp_footer a.wp_rp_backlink {
      display: none;
    }
