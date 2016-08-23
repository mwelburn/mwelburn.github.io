---
id: 17
title: Creating a Login Form via Bootstrap Navbar Dropdown
date: 2012-08-20T03:41:00+00:00
author: Michael Welburn
guid: http://michaelwelburn.com/2012/08/20/creating-a-login-form-via-bootstrap-navbar-dropdown/
permalink: /2012/08/20/creating-a-login-form-via-bootstrap-navbar-dropdown/
categories:
  - Javascript
  - Programming
  - Ruby on Rails
tags:
  - bootstrap
  - devise
  - form
  - JavaScript
  - jQuery
  - login
  - navbar
  - rails
  - Ruby on Rails
redirect_from:
  - /post/29805873466/creating-a-login-form-via-bootstrap-navbar-dropdown/
  - /post/29805873466/
---
Working on a new website for fun that leverages Twitter <a title="Twitter Bootstrap" href="http://twitter.github.com/bootstrap/" target="_blank">Bootstrap</a> (until the day comes that I can do any of the UI myself), I recently decided to try to integrate my login form with Twitter Bootstrap’s <a title="Twitter Bootstrap Navbar" href="http://twitter.github.com/bootstrap/components.html#navbar" target="_blank">navbar</a> <a title="Twitter Bootstrap Dropdown Javascript" href="http://twitter.github.com/bootstrap/javascript.html#dropdowns" target="_blank">dropdown</a> to eliminate the need for a separate login page. The code was pretty simple, following the documentation on the website and dropping in my Rails ERB form (I’m leveraging <a title="Devise Github" href="https://github.com/plataformatec/devise" target="_blank">Devise</a> for authentication).

<p style="text-align: center;">
  <img class="size-full wp-image-134 aligncenter" title="Bootstrap Login" alt="tumblr_m91anjsg5K1r1vsop" src="http://michaelwelburn.com/wp-content/uploads/2012/08/tumblr_m91anjsg5K1r1vsop.png" width="398" height="298" srcset="http://michaelwelburn.com/wp-content/uploads/2012/08/tumblr_m91anjsg5K1r1vsop.png 398w, http://michaelwelburn.com/wp-content/uploads/2012/08/tumblr_m91anjsg5K1r1vsop-300x224.png 300w" sizes="(max-width: 398px) 100vw, 398px" />
</p>

<!--more-->

The code below renders the image above, which for all intents and purposes looks great and works exactly how I imagined…except that the dropdown javascript code has an event that closes the dropdown when clicked. This behavior is not ideal when you are looking to fill out a form, and a click into one of the text fields closes your form.

    <a class=”btn dropdown-toggle” data-toggle=”dropdown” href=”#”>Login<span class=”caret”></span></a>
    <ul id=”dropdown-login” class=”dropdown-menu” style=”padding: 5px 10px 0px 10px;”>
       <%= form_for(“user”, :url => user_session_path) do |f| %>
          <li>
             <div class=”control-group”>
                <%= f.label :login, :class => “control-label” %>
                <div class=”controls”>
                   <%= f.text_field :login %>
                </div>
             </div>
          </li>
          <li>
             <div class=”control-group”>
                <%= f.label :password, :class => “control-label” %>
                <div class=”controls”>
                   <%= f.password_field :password %>
                </div>
             </div>
          </li>
          <li>
             <div class=”control-group”>
                <%= f.label :remember_me, :class => “control-label” %>
                <div class=”controls”>
                   <%= f.check_box :remember_me %>
                </div>
             </div>
          </li>
          <li>
             <%= f.submit “Sign in”, :class => “btn btn-primary” %>
          </li>
       <% end %>
    </ul>

Fortunately, it is pretty easy to stop this from occurring by stopping the click event from bubbling up. By creating a click handler on my “dropdown-login” tag, I was able to stop the propagation of the event, which allowed my form to work as desired.

    // Prevent closing the sign in form on click
    j$(‘#dropdown-login’).click(function(e) {
       e.stopPropagation();
    );
