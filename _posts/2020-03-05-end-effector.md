---
layout: post
title: "One Robot, Two End Effectors"
author: "Emily"
categories: updates
tags: [updates]
image: End_Effector_2.png
---

### Weed Removal End Effector Design

The goal of uproot robotics is to be able to remove weeds without the use of herbicides to make agriculture more sustainable. Through research into weed removal methods we discovered that by removing the top of the stem and the leaves most weeds will be unable to produce nutrients and wither. 

Therefore, we decided a rotating blade end effector design would be most effective in removing weeds. A motor chuck is used to enable the connection between a circular saw blade and a motor. A DC motor was chosen over stepper or servo as it allows for simple control and no positional control is needed. Through evaluation and testing of different motors, we were able to determine that higher rotational speed is more important than high torque for cutting weeds. The final motor spec has a no-load rpm of 19300 and a stall torque of 69.16 oz-in. 

The weed removal end effector design is shown below. 

![end1](/assets/img/End_Effector_1.png)

### Demo End Effector Design

For the demonstration at Symposium, it would be a risk to the public to have a rotating blade and it would be logistically difficult to have enough weeds to demonstrate the cutting capability. So for demo day, we designed an alternative end effector.  

We decided to switch the blade for a plastic servo motor arm, so viewers will still be able to see the end effector motor switching on and off. As the end effector will no longer be able to cut through anything, we designed simulation weeds that can be toppled over by the new end effector. Through testing we also realized we can use a significantly smaller motor for the demonstration which will save power throughout the day, so we switched to a smaller micro-metal motor. 

The demo end effector is shown toppling a "weed" in the following video. 

![end2](/assets/img/End-Effector-3.gif)
