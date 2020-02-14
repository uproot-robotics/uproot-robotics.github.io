---
layout: post
title: "Quick Update Before Reading Week"
author: "Aaron"
categories: updates
tags: [updates]
image: wide_angle_john.png
---

Just a quick update before we head out to reading week. We are all looking forward to a nice little break, but I think most of us are also planning to put in some work on the robot at some point during reading week.

## Wide-Angle == More Weeds!
The photo above featuring our personal model, John, was taken with our new wide-angle camera to be mounted on the bottom of the main motor platform. There will be more sample photos to come.

We realized that with our previous standard Raspberry Pi Module camera, the Field of View (FoV) was actually smaller than the entire range of the delta robot arm, operating at the desired height off of the ground. The new camera module, with a wide-angle lens (shown below) was decided on to give us non-limiting FoV. So far this has been proven to be more than sufficient, as the viewing angle of this camera is 160 degrees!

![partsrandom](/assets/img/wide_angle_camera.jpg)

After calibrating the camera spatially, to fix straight lines in the image, we are getting decent imaging results with the camera mounted on the robot. At this point, we just need to do some more testing and comissioning on the camera, and might have to write some more spatial mapping routines in the vision node to tie everything together. At that point, hopefully we can start rigourously testing the vision side of the software to make sure the protoype requirements are met.