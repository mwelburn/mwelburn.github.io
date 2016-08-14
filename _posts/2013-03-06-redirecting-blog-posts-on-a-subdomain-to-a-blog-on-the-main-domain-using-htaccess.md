---
id: 304
title: Redirecting Blog Posts on a Subdomain to a Blog on the Main Domain using .htaccess
date: 2013-03-06T19:31:41+00:00
author: Michael Welburn
layout: post
guid: http://michaelwelburn.com/?p=304
permalink: /2013/03/06/redirecting-blog-posts-on-a-subdomain-to-a-blog-on-the-main-domain-using-htaccess/
categories:
  - Website
tags:
  - htaccess
  - subdomain
  - tumblr
  - Wordpress
---
Back when I migrated over from <a title="Migrating from Tumblr to WordPress" href="http://michaelwelburn.com/2013/02/18/migrating-from-tumblr-to-wordpress/" target="_blank">Tumblr to WordPress</a>, I also made the decision to switch my blog from http://blog.michaelwelburn.com to http://michaelwelburn.com. Originally I had planned to put other stuff on the landing page and keep the blog separate, but I have since realized that using the main domain as a landing page to the blog is of much more use.

After spending (what I thought) was too much time on it during the migration, I ended up deciding that it probably wasn&#8217;t worth the effort considering I couldn&#8217;t imagine there would be many sites linking to the old blog. Unfortunately, I noticed some <a title="Blackberry Forums" href="http://supportforums.blackberry.com/t5/Web-and-WebWorks-Development/Best-weather-API/td-p/1955027" target="_blank">random Blackberry forum post</a> was linking to the subdomain, and it annoyed me that those poor guys were getting a 404 error, so I recently ended up trying to figure out why my use of 301 redirects didn&#8217;t seem to be working.

<!--more-->

My initial effort to map my old Tumblr post URLs to the new WordPress ones didn&#8217;t go as swimmingly as I had hoped. The URLs I had for Tumblr had a different structure than what I chose for WordPress, and I was able to use the <a title="Simple 301 Redirects" href="http://wordpress.org/extend/plugins/simple-301-redirects/" target="_blank">Simple 301 Redirects</a> plugin to <a title="Migrating from Tumblr to WordPress" href="http://michaelwelburn.com/2013/02/18/migrating-from-tumblr-to-wordpress/" target="_blank">map the changes</a>.

### Tumblr

http://blog.michaelwelburn.com/**post/12220032768**/comparing-weather-apis

### WordPress

http://michaelwelburn.com/**2011/11/02**/comparing-weather-apis/

While this was a manual process, I only had 10 to 15 posts to map. Unfortunately, this only seemed to work when the URLs were on the same domain. When I tested the Tumblr example URL above, it would simply show a 404 error page on http://michaelwelburn.com. I had set up a subdomain to redirect to the main domain in my DNS, so I couldn&#8217;t see why it wouldn&#8217;t redirect the subdomain to the main domain (maintaining the URL path), and then resolve the post based on that.

With renewed energy (a month later), I started playing around with my DNS records again, gave up, and then found this <a title="Redirecting a subdomain to the main domain" href="http://www.kingscooty.com/blog/redirect-a-subdomain-to-a-main-domain-using-htaccess/" target="_blank">post</a> that turned out to solve my problem. It turned out that I also needed to modify the .htaccess file to add the following 301 redirect from the subdomain.

> RewriteCond %{HTTP_HOST} ^blog.michaelwelburn.com$ [OR]
  
> RewriteCond %{HTTP_HOST} ^www.blog.michaelwelburn.com$
  
> RewriteRule (.*)$ http://www.michaelwelburn.com/$1 [R=301,L]