---
title: "Google Calendar Time Tracker"
date: 2020-06-13
draft: false
tags: ["projects"]
---

> The rest of this post was originally written in spring of 2019, when I'd just created the time tracker.  In the interim, Google seems to have changed their auth flow, and I can no longer get the tracker to work -- not that I tried very hard, in all honesty.  It's undocumented and hastily written, and I wouldn't necessarily recommend trying to reuse my code.  Nevertheless, I still like the idea!  Maybe I'll rewrite it some day...

Google Calendar Time Tracker is a simple command-line utility that displays time breakdowns for the tasks on my Google Calendar, grouped by calendar and optionally by name.  For example, I can compute the total time of the events in my calendar for 6.832 (Underactuated Robotics) for the past week, and also check what portion of this time was spent on homework or in class.

I wrote this to track how much time I spend on each of my classes, but it's flexible enough to be useful as a generic time tracker for contractors billing by the hour, interns working an hourly wage, or anyone else that wants to check how they're spending their time. You can find it on my github [here](https://github.com/shilohc/gcal-tracker ).

![Screenshot of Google Calendar with an inset showing the output of my time tracker](/img/gcal_tracker.png)

Above is my calendar for the week, with an inset showing some of the time tracker's output for this week's planned schedule.  
