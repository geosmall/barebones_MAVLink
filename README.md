# A Minimal QGroundControl MAVLink Example

[QGroundControl](http://qgroundcontrol.com/) is an open-source
"ground control station" for uncrewed aerial vehicles. It's an all-in-one center for display of live-streaming inbound UDP video, attractive display of vehicle telemetry, and manual joystick remote control. It communicates with the vehicle controller using the [open MAVLink protocol](https://mavlink.io/). Even though it's got a lot of specialized features for autonomous aerial vehicle control, I thought it would be an attractive option for remote human-piloted teleoperation of a simpler robot. 

However, I had a hard time finding clear documentation or a simple example of how to establish a MAVLink "connection" between a vehicle and [QGroundControl](http://qgroundcontrol.com/) outside of the context of a complicated UAV or UUV autopilot framework. 

The Arduino sketch here uses a few [common MAVLink messages](https://mavlink.io/en/messages/common.html) and auto-generated C headers (added to this project as a submodule) to negotiate a serial-port connection with QGroundControl. Once connected, the Arduino will send some simple telemetry to QGroundControl and respond to manual control packets from QGC if a [supported joystick](https://docs.qgroundcontrol.com/en/SetupView/Joystick.html#supported-joysticks) is connected to the ground control computer. I tested this with the Logitech F710.

This sketch is a demonstration and is not necessarily intended to be useful by itself. If you are working with a [quadcopter](http://px4.io/), [boat](https://discuss.ardupilot.org/t/rover-3-0-0-release/8267), [submarine](https://github.com/bluerobotics/ardusub/), or [wheeled rover](https://discuss.ardupilot.org/t/rover-3-0-0-release/8267), using one of the appropriate autopilot software offerings would probably be more appropriate than working with MAVLink messages directly. 

That said, I was looking to add MAVLink telemetry and control to a platform where the existing autopilot frameworks were a poor fit. If you're in this situation and want to successfully connect to QGC using the basic MAVLink C headers, you may find this example helpful.

## Using This Sketch

I was hoping to implement this on an Arduino Uno, but as-is it doesn't fit in memory. 
So far, I've tested this on a Teensy 3.6. I might try to optimize it to fit on the Uno but it will require more careful sharing of resources, and I don't want that complication in the basic demo.

Uploading this to anything Arduino-compatible (implementing the `Serial` methods, etc) and manually connecting to the appropriate serial port in QGroundControl (`Q Menu->Comm Links->Add`) should result in a connected vehicle state.

Connecting, calibrating, and enabling a compatible joystick, like my Logitech F710, will cause QGroundControl to start sending [`#69 MANUAL_CONTROL`](https://mavlink.io/en/messages/common.html#MANUAL_CONTROL) messages to the target controller. To simplify the hardware for this demonstration, the sketch just re-transmits the manual control message back to QGroundControl. The re-transmitted messages can be viewed in the MAVLink Inspector widget:

![](README_images/mavinspect.png)

## This is a work in progress.
