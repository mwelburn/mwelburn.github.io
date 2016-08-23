---
id: 44
title: Migrating from Tumblr to WordPress
date: 2013-02-18T08:00:59+00:00
author: Michael Welburn
guid: http://michaelwelburn.com/?p=44
permalink: /2013/02/18/migrating-from-tumblr-to-wordpress/
categories:
  - Website
tags:
  - feedburner
  - migration
  - tumblr
  - Wordpress
  - wordpress.com
  - wordpress.org
---
For a while now I&#8217;ve been wanting to move this blog from <a title="Tumblr" href="http://www.tumblr.com" target="_blank">Tumblr</a> to <a title="Wordpress" href="http://www.wordpress.com" target="_blank">WordPress</a>. I felt that WordPress allowed a lot more flexibility for me, and Tumblr felt a lot more like a media sharing website rather than catering to the large blocks of text I end up with. After evaluating the free WordPress.com vs the self hosted <a title="Wordpress.org" href="http://www.wordpress.org" target="_blank">WordPress.org</a> (here&#8217;s a <a title="Wordpress.com vs WordPress.org" href="http://en.support.wordpress.com/com-vs-org/" target="_blank">breakdown</a>), I decided that I wanted the ability to leverage the vast amount of WordPress plugins which was only available if you self host the installation.

Normally I would spend a lot of my own time figuring out how to install WordPress on my own Amazon AWS instance, set up all of the application servers, and generally spend 2 frustrated weeks trying to get everything right. This time I&#8217;ve decided I&#8217;d rather put that effort toward some sort of fun side projects instead. I&#8217;ve also been putting this move off for over half a year, so I figured I might as well just spend the least amount of time possible to make the move.

<!--more-->

### Finding a Web Host

I already had my domain registered with <a title="Namecheap" href="http://www.namecheap.com/" target="_blank">Namecheap</a>, and I ended up going with <a title="bluehost" href="http://www.bluehost.com" target="_blank">bluehost</a> for my hosting for a couple reasons. Along with a lot of other hosting websites, they offer a one click installation to get a WordPress instance up and running really quick. They also were the cheapest &#8220;reliable&#8221; host when I factored in possibly hosting multiple domains.

In hindsight, perhaps the first thing I should have done after signing up with bluehost was <a title="DNS" href="https://my.bluehost.com/cgi/help/1" target="_blank">updating</a> my DNS records to point to bluehost (via the bluehost console). I made the mistake of saving that part until the end (of my first attempt), and had a ton of issues with the reference to the blog being half domain name and half IP address. From doing some <a title="http://wordpress.org/support/topic/trying-to-change-url-from-ip-to-domain-name" href="http://wordpress.org/support/topic/trying-to-change-url-from-ip-to-domain-name" target="_blank">reading</a>, it sounded like if I went directly into the database and replaced every reference to the IP address with my domain, as well as every file on the server, it might work. However, after an hour or two of no positive resolution, I gave up. In the end I just deleted my first attempt and tried again &#8212; the second time it went very smoothly.

Once my second attempt was up and running, I had a laundry list of things I needed to accomplish.

### Migrating Posts from Tumblr

I referenced what was probably the first google <a title="Move your Tumblr blog to WordPress" href="http://techinch.com/blog/move-your-tumblr-blog-to-wordpress" target="_blank">result</a> for this migration, and it was pretty spot on. The migration ended up being pretty easy, as there was a some built in support to <a title="Import from Tumblr in 3 easy steps" href="http://en.blog.wordpress.com/2012/02/02/import-from-tumblr-in-3-easy-steps/" target="_blank">import</a> posts directly from a Tumblr account. Following the directions (most importantly, removing the custom domain from your Tumblr account) brought these over easily. One thing to note is that posts I had queued for publish in the future were not brought over (as they weren&#8217;t considered published or drafts), so I had to move them back to a draft to bring them over.

Additionally, all of those blog posts now referenced Tumblr images (as images uploaded when writing posts on Tumblr are hosted there). Maybe more out of paranoia than anything else, I manually replaced all the links in the WordPress posts with re-uploaded pictures onto my server.

### Migrating Comments from Disqus

I spent a little bit of time determining whether to stick with <a title="Disqus" href="http://disqus.com/" target="_blank">Disqus</a> for commenting, or leveraging the out of the box WordPress comments. In the end, I just stuck with WordPress comments. However, there didn&#8217;t seem to be a good tool to do this migration (Disqus comments against a Tumblr post that doesn&#8217;t map to a WordPress post directly). I only have about 10 comments anyway, so it is something I just will be manually creating the comments directly in the database via <a title="phpMyAdmin" href="http://www.phpmyadmin.net/" target="_blank">phpMyAdmin</a> (available as part of bluehost).

Disqus allows exporting comments from their system, which I plan to use to get some details like comment date that don&#8217;t explicitly appear in the UI. Unfortunately, I have yet to receive any notification that they have been exported yet, so I haven&#8217;t been able to complete this task.

### Redirecting a Subdomain to the Primary Domain

I had Tumblr on a subdomain of michaelwelburn.com, and now I wanted to move the WordPress instance to the primary domain. Not only did that mean I would lose my subdomain links, but Tumblr&#8217;s URL structure was different from the one I set up for WordPress.

The first thing I needed to do was to get blog.michaelwelburn.com to redirect to michaelwelburn.com. I had assumed that I could accomplish this using a CNAME record in the bluehost domain manager to point to michaelwelburn.com, but that just showed a bluehost error page. It turned out I needed to set up a <a title="Subdomain" href="https://my.bluehost.com/cgi/help/361" target="_blank">subdomain</a> to the site and point it directly to the public_html folder.

**UPDATE: **I had additional success modifying the .htaccess file to redirect the subdomain AND get individual post redirects to work <a title="Redirecting Blog Posts on a Subdomain to a Blog on the Main Domain using .htaccess" href="http://michaelwelburn.com/2013/03/06/redirecting-blog-posts-on-a-subdomain-to-a-blog-on-the-main-domain-using-htaccess/" target="_blank">here</a>.

### Redirecting Tumblr URLs

While I had limited success pointing a subdomain to the primary domain and then doing a 301 redirect (or rather, I don&#8217;t have enough posts linked on the internet for it to really matter), I was able to use a WordPress plugin called <a title="Simple 301 Redirects" href="http://wordpress.org/extend/plugins/simple-301-redirects/" target="_blank">Simple 301 Redirects</a> to do some redirecting.

<img class="alignnone size-full wp-image-171 aligncenter" alt="Simple 301 Redirects" src="http://michaelwelburn.com/wp-content/uploads/2013/02/Screen-Shot-2013-02-17-at-9.51.32-PM.png" width="579" height="166" srcset="http://michaelwelburn.com/wp-content/uploads/2013/02/Screen-Shot-2013-02-17-at-9.51.32-PM.png 579w, http://michaelwelburn.com/wp-content/uploads/2013/02/Screen-Shot-2013-02-17-at-9.51.32-PM-300x86.png 300w" sizes="(max-width: 579px) 100vw, 579px" />

On the topic of WordPress post URLs, I also gave some consideration to removing the date structure from my post URLs for readability, but came across quite a few people who <a title="Don't Use Postname" href="http://digwp.com/2011/06/dont-use-postname/" target="_blank">claim</a> it&#8217;s not a good idea. No big deal.

### Updating RSS Feed

Tumblr allowed using <a title="feedburner" href="http://feedburner.google.com/" target="_blank">feedburner</a> for RSS feeds, which is a website that allows you to generate metrics on your subscribers (among other things). One of the nice things about using a service like feedburner is that I was able to point the feedburner RSS feed I set up from my Tumblr page over to the WordPress page, and none of my 5 subscribers would even know.

Additionally, I needed to set up feed <a title="feedburner autodiscovery" href="http://labnol.blogspot.com/2006/05/feedburner-tutorial-replace-your-blog.html" target="_blank">autodiscovery</a> to allow users to type in the domain into their RSS Reader of choice and have it find the feedburner feed. To do this, I added the following two lines inside the <head> tag of header.php:

    <link rel="alternate" type="application/atom+xml" title="Atom" href="http://feeds.feedburner.com/michaelwelburn/QpRL" />
    <link rel="alternate" type="application/rss+xml" title="RSS" href="http://feeds.feedburner.com/michaelwelburn/QpRL">

### ****Random Notes

There ended up being a couple of odds and ends that I needed to deal with as well.

  * I noticed that Tumblr used <!&#8211; more &#8211;> for their blogs to denote the Read More mark, but WordPress appeared to require <!&#8211;more&#8211;> instead. I had to manually go into each blog post and change this.
  * I wanted to find a backup solution (I&#8217;m always paranoid). There happened to be a <a title="Wordpress Backup to Dropbox" href="http://wordpress.org/extend/plugins/wordpress-backup-to-dropbox/" target="_blank">plugin</a> to back up the entire WordPress directory and the database to Dropbox.
  * Most of the free themes on WordPress seemed pretty ugly. After looking around briefly on some different theme selling sites, I hadn&#8217;t found anything that really blew me away. I ended up finding this particular theme that I liked enough to stick with for now, and made a few CSS tweaks to fix some parts I found strange.

* * *

Overall, I&#8217;m pretty happy that I finally made the switch. I felt like I wanted to get some experience setting up a WordPress blog, and am enjoying the crazy amount of customizing that I can do to it. Perhaps one day, when I find myself in a &#8220;web design&#8221; craze, I&#8217;ll spend some time actually redesigning this site.
