---
id: 804
title: Rails 4 Nested Object Creation and Parent Presence Validation Error
date: 2014-02-10T06:00:21+00:00
author: Michael Welburn
layout: post
guid: http://michaelwelburn.com/?p=804
permalink: /2014/02/10/rails-4-nested-object-creation-and-parent-presence-validation-error/
categories:
  - Programming
  - Ruby on Rails
tags:
  - Ruby on Rails
---
As I haven&#8217;t used Rails in a year or two (and even then, only a few small <a title="Introducing Weather Wardrobe: The quickest way to get dressed in the morning" href="http://michaelwelburn.com/2013/02/27/introducing-weather-wardrobe-the-quickest-way-to-get-dressed-in-the-morning/" target="_blank">side projects</a> to learn the language for fun) I tend to run across your typical &#8220;new to the language&#8221; syntax nuances relatively frequently. Typically a quick Google gets me an answer and I can move on (can&#8217;t imagine what debugging was like 20 years ago&#8230;) but this past weekend I was running into an issue while building a relatively simple time tracking application in Rails that I thought might be helpful for anyone else starting up.

<!--more-->

This particular use case revolved around creating Timesheets and the Time Entries that fall within them. In this case, Timesheet can have many Time Entries, but we want to ensure that a Time Entry belongs to a Timesheet. Below are my original models (stripped of any of the implementation irrelevant to this post).

    class Timesheet < ActiveRecord::Base
      has_many :time_entries
    end

    class TimeEntry < ActiveRecord::Base
      belongs_to :timesheet
      validates :timesheet_id, :presence => true
    end

I then went about building a simple HTML page that allowed editing of both the Timesheet and the related Time Entries in a single form, and upon submission would create both the parent and child with a single **save** invocation on the Timesheet. Below is the simplest form of the controller and html.erb file.

    class TimesheetsController < ApplicationController
      def new
        @timesheet = @user.timesheets.new
        2.times do # Testing with 2 empty rows
          @timesheet.time_entries.build
        end
      end
    end

    <%= form_for(@timesheet) do |f| %>
      <%= f.date_select :start_date %>
      <%= f.date_select :end_date %>
    
      <%= f.fields_for :time_entries do |entry| %>
        <%= entry.date_select :completion_date %>
        <%= entry.text_field :notes %>
      <% end %>
    
      <%= f.submit "Submit" %>
    <% end %>

However, when I would click submit the creation of the child Time Entry records were failing.

> ActiveRecord::RecordInvalid: Validation failed: Time entries timesheet can&#8217;t be blank

The implication here was that the timesheet_id field on the Time Entry was not being set implicitly during the save context on the timesheet.

After searching around a bit (more accurately, trying to find the right words to Google), I came across a StackOverflow <a title="Trouble with Foreign Key" href="http://stackoverflow.com/questions/13344059/trouble-with-accepts-nested-attributes-for-on-validating-foreign-key" target="_blank">post</a> that got me on the right track. The issue was that I was trying to validate that the foreign key on Time Entry for Timestamp existed using **validates_presence** rather than using **inverse_of** in the relationship declaration. The official documentation around the best practice is <a title="Validating the Presence of a Parent Model" href="http://api.rubyonrails.org/classes/ActiveRecord/NestedAttributes/ClassMethods.html#label-Validating+the+presence+of+a+parent+model" target="_blank">here</a>.

After making the tweak to my object model, I finally was successful.

    class TimeEntry < ActiveRecord::Base
      belongs_to :timesheet, :inverse_of => :time_entries
    end