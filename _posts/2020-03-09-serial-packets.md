---
layout: post
title: "A Killer Serial Setup"
author: "Eric"
categories: updates
tags: [updates]
image: serialview.png
---
This post is actually about something we implemented a while back when we first transitioned to the dual controller architecture (Jetson + Teensy), but I wanted to share to give some insight into our serial communications setup. 

## High Level Arch
In order to decouple the motor control and hardware from our main compute module (the Jetson Nano), we had decided to use a Teensy 3.2 to handle all motor and sensor interfacing and to control the Teensy from the Jetson. A high level view of this architecture including the ROS architecture on the Jetson is seen here:

![SW Arch](/assets/img/SW_ARCH_REVISED.png)

## Defining Our Serial Packet
In order to accomplish this smoothly, we created a git repo with our Serial Packet definitions and then used that repo as a submodule within our Teensy and our Jetson repos. We defined a simple message struct, packed into a union (in order to easily change to an array of bytes to send).

```c++
enum CmdType {
    CMDTYPE_MTRS = 0,    // Actuate motors
    CMDTYPE_CAL,         // Calibrate motors
    CMDTYPE_ENDEFF_ON,   // Turn ee on
    CMDTYPE_ENDEFF_OFF,  // Turn ee off
    CMDTYPE_CONFIG       // Set speed / accel
};

struct CmdMsg {
    /* Type of message */
    CmdType cmd_type;

    /* Data to send to Teensy */
    int is_relative;
    int mtr_angles[MAX_SUPPORTED_MTRS];
    int mtr_speed_deg_s;
    int mtr_accel_deg_s_s;

    /* Data to be received from Teensy */
    int cmd_success;
};

union Packet {
    CmdMsg msg;
    char bytes[sizeof(CmdMsg) + 1]; // + 1 for delimiter
};

```

The general flow is as follows:
1. Jetson creates CmdMsg -- Packet and writes over serial
1. Teensy unpacks the received bytes (uses delimiter \n between messages)
1. Teensy uses the CmdType parameter to decide on action
1. Teensy validates command with response, matching the original message but with cmd_success as true

This simple Serial setup allows us to have an easily configurable solution for controlling the motors. In future we would ideally like to add more robustness by using CRC or similar error checks.

