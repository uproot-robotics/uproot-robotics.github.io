---
layout: post
title: "Testing Tracking Software"
author: "Aaron"
categories: updates
tags: [updates]
image: arm_over_bed.jpg
---

As the final stages of commissioning and testing are fast approaching, the system software is being tuned for our specific application. The camera module with fisheye lens attached, as mentioned in a previous post, has been successfully mounted on the underside of the platform.

<img src="/assets/img/fisheye_mounted.jpg" alt="drawing" width="400"/>

At this point, the Vision and Tracking software modules are being rigorously tested.

## Changing the Strategy
Until this point, static imaging and targeting of weeds was being performed to bring-up the electro-mechanical system. With these components working, the tracking software is being changed to instead do dynamic tracking (i.e. when the system is on the move), with a continuous update of arm angles sent to the controller. The idea is that the system will function the same way on the move as it does statically, except that it will continually update target locations. A sample of testing at an intermediate stage is shown in the video below!

<iframe width="560" height="315" src="https://www.youtube.com/embed/jqSeTDt2_OY" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>