---
layout: post
title: "Dismantling ARToolkit Part 2: Contour Detection"
description: ""
category: 
tags: [libkoki]
---
{% include JB/setup %}

What on earth happened to part 1, I hear you ask? Allow me to explain.

This summer, I have been given the chance to do an internship to help advance the [Student Robotics](https://www.studentrobotics.org/) kit, specifically its vision.
My task is to get the robots seeing [fiducial markers](http://en.wikipedia.org/wiki/Fiduciary_marker) in the arena instead of the current colour-based system.
It was decided that the best route to take would be to write the application from scratch, helping us meet our requirements of maintainability, robustness and speed.
To do so, we’ll look at how [ARToolkit](http://www.hitl.washington.edu/artoolkit/) works.
This rather unpleasant task was started by [Rob Spanton](https://xgoat.com/), another Student Robotics member, who just so happens to be supervising me this summer.
Rob wrote [Dismantling ARToolkit Part 1: Labelling](https://xgoat.com/wp/2011/05/08/dismantling-artoolkit-part-1-labelling/) a while ago, so this is me continuing his work.
If you haven’t already read his [blog post](https://xgoat.com/wp/2011/05/08/dismantling-artoolkit-part-1-labelling/), may I gently recommend you do so.

Contours
--------

So what do we mean by the word *contour*? [Wiktionary](http://en.wiktionary.org/wiki/contour) offers two definitions:

 1. An outline, boundary or border, usually of curved shape.
 2. A line on a map or chart delineating those points which have the same altitude or other plotted quantity: a contour line or isopleth.

Any readers familiar with map- or chart-based navigation will perhaps be more used to the latter definition; that was certainly the case for me. In the context of marker detection, the former is the more appropriate definition. It is the prominent black border of a marker that is detected using the technique discussed here.

![Sample ARToolkit Marker](http://media.tumblr.com/tumblr_lxrjxl8fUo1r5fb5j.png "A sample ARToolkit Marker")

In simple terms, the technique is as follows: starting at some vertex, walk clockwise around the marker chaining together the pixels discovered until one ends up back at the starting vertex. The outline of the marker has then been traced. But how exactly does one *walk* around the marker?


Moore-Neighbor Tracing
----------------------

This method of contour detection is known as [Moore-Neighbor Tracing](http://en.wikipedia.org/wiki/Moore_neighborhood). The code to do so in ARToolkit can be found in *arDetectMarker2.c* in the function *arGetContour(…)*. The steps taken in said function are described below.

1. A clip region is one of the outputs from *labeling2(…)* which stores the minimum and maximum *x* and *y* co-ordinates. One cannot simply assume that the top left pixel is a vertex of the marker — the maker could be rotated about any of its three axes, distorting the image. All that is known is that there is a vertex somewhere on the top row of the clipped region. A linear search, moving from left to right, will reveal this. Consider this vertex V<sub>start</sub>.

2. Set P<sub>current</sub> = V<sub>start</sub>. Now consider the pixel directly above P<sub>current</sub>; let’s call it P<sub>check</sub>.

3. If P<sub>check</sub> is not black (i.e. its label is zero), set P<sub>check</sub> as the pixel one step round in a clockwise direction, about P<sub>current</sub>. Repeat until a black pixel is found. Consider this black pixel as P<sub>current</sub> and add it to the chain.

4. If P<sub>current</sub> == V<sub>start</sub>, stop. Otherwise…

5. Set P<sub>check</sub> as the previous pixel (i.e. the one just added to the chain), then advance it one clockwise step about P<sub>current</sub>. Go to 3.

Assuming the blob that’s being checked is a marker, the border of said marker has then been traced. Ensuring the traced blob is a marker occurs at a latter stage and will be the subject of another blog post.

After this occurs in *arGetContour(…)* the chain array is then re-ordered, moving all pixels identified beyond the furthest pixel from V<sub>start</sub> to the front of the array and adds the necessary link to complete the contour. Right now, I have no idea why this is done. One can only assume it helps at a later stage in the program.

My next challenge: work out how ARToolkit ensures the blobs are actually squares.
