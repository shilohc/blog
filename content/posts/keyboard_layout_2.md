---
title: "Custom Keyboard Layout, Part 2"
tags: ["keyboards"]
date: 2020-06-16
lastmod: 2020-06-16
draft: false
---

Okay, so my earlier [post]({{< ref "/posts/typewriter_keyboard" >}}) about modifying your keyboard layout sold you, but you don't want to buy a whole new keyboard just yet?  You want a software-side key remapping solution?  Well, sure, fair enough.

In the long term, this will be more annoying than modifying the keyboard firmware -- as I mentioned before, if you modify the keyboard firmware then you get your layout everywhere you plug in the keyboard, but software-side solutions are specific to the computer you're on.  However, software-side solutions are easier to get started with.  Also you probably don't want to change the firmware on the keyboard embedded in your laptop (for example, if you want to remap capslock to escape, which is one of the earliest items in my new computer setup checklist).

On Linux: Augh.  If you're using X you can use xkb, for which there are shortcuts to do common modifications like e.g. swap capslock and escape; if you want to do anything more complex be prepared to spend a few hours on it.  I don't use Wayland so I did a quick duckduckgo about it and my eyes went to hell.

On Mac: In later versions of MacOS, System Settings -> Keyboards deigns to allow you to perform a shortlist of modifications (e.g. swap capslock and escape).  For anything more advanced you'll want Karabiner, which I seriously can't recommend enough.  Comes with two tools: Karabiner Elements (set your remappings) and Karabiner Element Viewer (shows you what input events your keyboard is sending, which is great for debugging keyboard firmware).

On Windows: Use AutoHotKey.  I don't know shit about AHK because the last* Windows computer I ever used was the desktop my dad helped me build when I was like 9 that dual-booted Windows XP and the educational edition of Ubuntu.  However, Hillel Wayne has a [great post about it](https://www.hillelwayne.com/post/ahk/) that you might want to refer to.

*This is not quite true.  Briefly, at a past internship, I had to use a Windows 10 computer because I was using a motion-capture system and its software was Windows-only.  I hated it.  Cortana kept yelling at me??  Things were happening seemingly uncontrollably?  How does anyone put up with this?
