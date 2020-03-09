---
layout: post
title: "A Quick Port to Add Acceleration Control"
author: "Eric"
categories: updates
tags: [updates]
image: arm_move.svg
---
At this point our delta arm is moving pretty well, but as of this point we have no acceleration control and we run at constant speed. If we want to make our motion as smooth as possible we need a way to add this level of control. Thankfully, there already exists a library that handles acceleration and velocity control of stepper motors!

## The AccelStepper Library
The library we're going to port to work with our setup is the [AccelStepper Library](https://www.airspayce.com/mikem/arduino/AccelStepper/). The idea here is to extend this library with our own wrapper class in order to override the low-level implementation while keeping the acceleration control. A highly simplified UML diagram shows the relationship between our wrapper, the AccelStepper class, and the [HighPowerStepperDriver](https://github.com/pololu/high-power-stepper-driver-arduino) class used to interface with our drivers.
![UML](/assets/img/accelstepper_uml.png)

## Making the Port
There are two bits of functionality to override - the step() functionality, which should now redirect to the HighPowerStepperDriver implementation, and the distanceToGo() functionality in order to use encoder readings. Also, the encoder change breaks the implementation of the runSpeedToPosition() function in the AccelStepper class, since we don't need it we'll just delete it.

Further, the AccelStepper class is intended to be extended which is why the core functionality (step, enableOutput, etc) is virtual. It also offers a constructor option AccelStepper::FUNCTION to indicate it is only being used for the acceleration control. This leaves our wrapper header looking something like this (included implementations here for simplicity):
```c++
/* Class to Control Stepper Motors -- Uses AccelStepper */
class StepperMotor : public AccelStepper {
public:
    StepperMotor() : AccelStepper(AccelStepper::FUNCTION) {};
    ~StepperMotor();

    void step(long) override;  // from accelstepper
    long distanceToGo();       // from accelstepper
    bool run();                // from accelstepper
    bool runSpeedToPosition() =
        delete;  // from accelstepper -- encoder override breaks this

private:
    HighPowerStepperDriver *m_hpsd;

    /* Rest of class definition
     * ...
     */
};
```

With our basic implementation of these looking something like this:
```c++
long StepperMotor::distanceToGo() {
    current_steps = enc_to_step(m_enc->read()));
    return targetPosition() - current_steps;
}


void StepperMotor::step(long) {
    if (m_hpsd->getDirection() != _direction)
        m_hpsd->setDirection(_direction);
    m_hpsd->step();
}

```

And we now have a highly simplified class to control our Stepper Motors with acceleration and speed control!

![Alt Text](/assets/img/acceleration.gif)
