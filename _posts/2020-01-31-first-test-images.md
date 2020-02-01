---
layout: post
title: "First Mounted Test Images!"
author: "Aaron"
categories: updates
tags: [updates]
image: test_images/middle_under_1.png
---
The initial stage of the Delta Arm is up and running. Controls software is running off of the Teensy controller, and the Inverse Kinematics running on the Nvidia Jetson Nano is interfacing with the controls software to move the delta arm. 

## Vision Testing
At this point, we are starting to capture some real test images from the camera mounted on the motor platform. In the image above the arm is fully extended. Another test image is shown below, where the arm is fully contracted:

![capture_2](/assets/img/test_images/middle_under_2.png)

Although the Delta arms can be seen in the test images, they do not appear to obscure the field of view too much. Configuration on the vision side will have to be played around with to keep this detection algorithm as robust as possible.