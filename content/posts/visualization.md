---
title: "Visualize Everything"
date: 2020-08-11
lastmod: 2020-08-11
draft: false
---

One of the best things you can do for yourself, debugging-wise, is visualization -- this is especially true in robotics, where we deal nearly exclusively with things like:

* the physical world
* cameras and other sensors
* algorithms like localization, mapping, and path planning
* humans and their activities
* ... and other stuff with physical meaning that you can look at

In all of these cases, it's much easier to understand what's going on if you slap your bizarre data that makes no sense in a picture, plot, or 3D visualizer.  It's easy to draw on images with OpenCV.  It's easy to publish stuff to rviz.  Unfortunately, it feels a lot harder than just printing a bunch of floats and trying to do the mental math yourself to check if they make sense.  This post is partially a reminder to me: it takes like 10 minutes tops to add a debug visualization; you are bad at mental math and after 10 minutes of trying to do it you will not have nearly as much insight as the visualization will give you.

Important point: the whole purpose of debug visualizations is that you have a confusing thing and you want to look at a picture of it, with as few conversion layers as possible so there's no room for bugs to sneak in.  For example, if you're trying to debug the conversion between image coordinates and some other quantity[^1], you don't want to convert your thing _back_ to image coordinates to draw it on an OpenCV image, since the conversion is what's not working in the first place.  Instead, try rviz if your output quantity is 3D points, or matplotlib if it's two-dimensional.

## Reference

* [Documentation for OpenCV draw functions](https://docs.opencv.org/2.4/modules/core/doc/drawing_functions.html )
* Documentation for the `visualization_msgs` [package](http://wiki.ros.org/visualization_msgs ) and the `visualization_msgs/Marker` [message](http://docs.ros.org/api/visualization_msgs/html/msg/Marker.html )
* Debugging over SSH, or for some other reason can't use rviz?  Try [rosshow](https://github.com/dheera/rosshow )
* If you happen to be using a RealSense, the Intel RealSense SDK also comes with a terminal-based tool for [visualizing depth data](https://dev.intelrealsense.com/docs/rs-depth )
* The usual debug flow using images in ROS is to publish images with debug info drawn on them to a new `xyz_debug` image topic, then view them using [`image_view`](https://wiki.ros.org/image_view ) or [`rqt_image_view`](http://wiki.ros.org/rqt_image_view )
* The most basic possible usage of matplotlib to plot two-dimensional data, here given as `xs` and `ys`:

{{< highlight python >}}
import matplotlib.pyplot as plt
plt.plot(xs, ys)
plt.show()
{{</ highlight >}}

See the docs for [`plot`](https://matplotlib.org/3.2.1/api/_as_gen/matplotlib.pyplot.plot.html#matplotlib.pyplot.plot ) for more info, particularly about format strings.

[^1]: I swear I've lost years of my life to image-related conversions.  For some reason nobody can agree on where the X and Y axes should go, what rows and columns and width and height mean, and what order the R, G, and B should go in.  Today for my internship I wrote a function that converts from [REDACTED] image coordinates to OpenCV image coordinates, and the X and Y axes are swapped but also for some reason one of them is backwards?  I'm only confident I got it right because the pink circle I'm drawing on the output image is in the correct place.  At least when you forget to convert BGR <-> RGB it's pretty immediately noticeable.

