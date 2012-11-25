---
layout: page
title: libkoki
header: libkoki
---
{% include JB/setup %}

libkoki is an open source C computer vision library for finding and estimating the position of libkoki markers in an image.
Unlike [ARToolkit](http://www.hitl.washington.edu/artoolkit/)-based fiducial marker systems (and similar) that find the best match from a list of known markers, libkoki markers use a code grid that contains both error detection and correction.
ARToolkit has problems with false positives, whereby markers are *seen* even when they are not present.
libkoki's error detection and correction significantly improves on this.

libkoki was initially developed for use by [Student Robotics](https://www.studentrobotics.org), who run an anual autonomous robotics competition for 16-18 year olds around Europe.
The competition arena has numerous markers around it, identifying boundaries and items of interest, and the robots use the information from libkoki to plan their movements.


Features:

 * 200+ unique codes available for use
 * world position estimation, given a marker of known size
 * Operates on OpenCV's IplImage, a common format
 * V4L2 helper functions for streaming from webcams
 * Error correction using [Hamming(7,4) codes](http://en.wikipedia.org/wiki/Hamming%287,4%29), and error detection using [CRC12](http://en.wikipedia.org/wiki/Cyclic_redundancy_check)
 * Operates on a frame-by-frame basis, so both video and still images can be used
 * Python bindings


Source
------

libkoki, the C library:

    git://srobo.org/libkoki.git

pykoki, the ctypes-based Python bindings for libkoki:

    git://srobo.org/pykoki.git

kokimarker, the Python library for generating libkoki markers:

    git://srobo.org/kokimarker.git


You can download the above by *cloning* the URIs above, e.g. `git clone git://srobo.org/libkoki.git`.
Note that [Git](http://git-scm.com) will be required to do so.

Until this page is expanded, feel free to [contact me](/about/) if you need any help.


Videos
------

The [Student Robotics](https://www.studentrobotics.org) 2012 Final:

<object width="560" height="315"><param name="movie" value="http://www.youtube.com/v/nzrsJhVAH7M?version=3&amp;hl=en_GB"></param><param name="allowFullScreen" value="true"></param><param name="allowscriptaccess" value="always"></param><embed src="http://www.youtube.com/v/nzrsJhVAH7M?version=3&amp;hl=en_GB" type="application/x-shockwave-flash" width="560" height="315" allowscriptaccess="always" allowfullscreen="true"></embed></object>
