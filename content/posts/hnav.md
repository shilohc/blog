---
title: "H-NAV"
date: 2020-06-12
draft: false
tags: ["projects"]
---

The H-NAV is a LIDAR-based navigational aid for visually impaired people. The LIDAR detects obstacles within two meters that are approximately at face height, and a set of vibrating motors in the hatband indicates the obstacles' direction and rough distance.

This project went through three major versions, each revision prompted by user tests. I tested the first two versions on myself and other sighted users, and was also able to test the third version on a group of blind volunteers.

![Picture of the H-NAV.  It's a light brown cowboy hat with a LiDAR mounted on the top and a PCB with a knob and slider on the side.  It is sitting on a white foam display head.](/img/hnav.jpg)

Pictured above is the third revision of the H-NAV. A PCB mounted on the side of the hat contains a slider for adjusting the obstacle distance threshold. The main PCB, tucked under the LIDAR, contains an ATMega324 microprocessor for processing LIDAR data and the lithium-ion battery charging circuitry, and the vibrating motors are mounted to a flexible PCB inside the hatband containing a second microprocessor, the ATTiny2313.

The H-NAV was my science fair project from 2013 to 2015, and won numerous awards, including Project of the Year at the California State Science Fair, National Finalist at the Junior Science and Humanities Symposium, and Regional Finalist for the Americas at the Google Science Fair. It was also featured in Popular Mechanics (winner of a Next Generation Breakthrough Award) and on the Today Show (winner of a Make the Future Award). 

{{< video "/img/todayshow.mp4#t=290" >}}

> {{< collapse "Transcript of relevant segment, 4:50 - 6:05" >}}

HOST: Let's move over to our last one.  This is Shiloh, she's 15, from Sunnyvale, California.  Shiloh, what did you invent here?  

SHILOH: So, it's a hat for the blind that detects face-level obstacles, and indicates them to the user using an array of vibrating motors inside the hatband.  

HOST: May I try one on?

SHILOH: Sure, why don't you try this on.  

HOST: Oh, you're going to let me try yours?  Okay.  Hey, give me an idea of how this works.  I have a giant head, and there's nothing I can do about that, but --

[LAUGHTER]

HOST: Okay.  So what -- how does it work.  

SHILOH: So you're going to start feeling vibrating motors all around your head --

HOST: Oh my gosh, I do.

SHILOH: -- corresponding to where the obstacles are.  

HOST: I sense you over here.  

SHILOH: So if you can feel my hand over here, you should be able to find -- feel it moving towards your front.  

HOST: That is remarkable.  

SHILOH: And these LEDs I put on this hat for demonstration also show the vibration pulses of the motors.  

HOST: How was this idea born?  How'd you come up with it?  

SHILOH: So that's kind of an interesting story.  One day I was demonstrating one of my robots to some children and I used an analogy like, a robot is blind until you put sensors on it, to explain how sensors are used.  And then a while later I was attending a presentation given by Brian Higgins, a blind rehabilitation specialist, and he was describing his idea for a robot guide dog, and I was thinking, why don't we put sensors on the blind so they can navigate like robots?  

HOST: It's brilliant, and I sense you right now, it's vibrating.  Congratulations Shiloh, it's great.  

{{< /collapse >}}
