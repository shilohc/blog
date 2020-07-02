---
title: "Handle Detector"
date: 2020-07-01
lastmod: 2020-07-01
draft: false
tags: ["projects"]
---

Last fall, I took [6.881 Intelligent Robot Manipulation](http://manipulation.csail.mit.edu/Fall2019/ ) (well, actually 6.881 is a "Special Subjects" course number, and when this course is offered again it'll probably be renumbered, but that was the number I took it under).  We spent a lot of time on perception, because the first step in a manipulation problem is figuring out what the hell you're looking at and where to grab it.  Often, you may want to grab something by the handle, which means you need to identify where the handle is on the object, which is where my final project comes in.

Given a segmented object point cloud (a point cloud with just the object, and no table or other clutter that may be in the environment), I fitted [quadrics](https://en.wikipedia.org/wiki/Quadric ) to randomly selected regions of the object, then used some simple heuristics to decide if the quadric probably represented a handle.  I used a recent algorithm for approximate quadric fitting that makes use of the object's surface normals, so the actual quadric fitting step is very fast, at the price of needing to take the time to find surface normals (which a real perception pipeline will likely need anyway for other stuff).  I tested it on some objects with handles from the [YCB object dataset](http://www.ycbbenchmarks.com/object-set/ ) and it works okay.  It didn't do great on the mug and drill objects (which both have cylindrical areas that aren't handles and non-quadric-shaped handles), but did pretty well on simple objects with cylindrical or ellipsoidal handles, even when I added noise to the point clouds.  Also, no machines were learned in the making of this project (I get the impression that ML would be the more "typical" way to do handle detection these days, but that has all the pitfalls of, uh, ML).

If you want to read more, the paper I wrote about this project is {{< res "*6881_paper*" "here," >}} and it has all my math, citations, test results, and pretty pictures.  Go look at the pretty pictures!

The full course materials for 6.881 from Fall 2019 are available online [here](http://manipulation.csail.mit.edu/Fall2019/ ), and the currently-in-progress notes are available [here](http://manipulation.csail.mit.edu/ ).  Prof. Tedrake also teaches the excellent 6.832 Underactuated Robotics, and his notes for that class are online [here](http://underactuated.csail.mit.edu/ ).  If you're interested at all in either of these topics, I strongly recommend you read his notes!  They're really good!

