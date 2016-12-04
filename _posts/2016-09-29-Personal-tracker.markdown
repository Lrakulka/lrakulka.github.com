---
layout: post
title:  "Personal tracker"
date:   2016-09-29 00:00:00 +0100
categories: IoT Android Samsung ARTIK Cloud
---

![Cover Image](/images/CoverImage.jpg)

**About this project**

I have created a system which uses smartphone to track people. It can be useful for next purposes: tracking children, workers, grandparents

**Goal**

This project was created for tracking how people work. For example we need a worker to distribute the add to people. But we can't follow him. That's why we can't be sure that the work was properly done.

For such goals I created this system which can remotely take a photo or a video on a worker's personal smartphone.

**Principle of the system**

One smartphone (controller) uses ARTIK Cloud to send actions to another smartphone (tracker). After obtaining of the action (makePhoto/ setOnVideo/ setOffVideo) from the controller, the tracker executes it and sends message result of the successful execution to the controller. Also the tracker sends coordinates of the location to ARTIK Cloud. When the coordinates of the location are inside of the area which is set by the user of the controller device, ARTIK Cloud sends a letter to an email of user's controller.
![Illustration of system](/images/System.jpg)
**ARTIK Cloud Configuration**

The first step is to create Device Type and Manifest. How to create Device Type your can read [here](https://developer.artik.cloud/documentation/tutorials/an-iot-remote-control.html#create-and-connect-a-new-device-type).

![Manifest](/images/Manifest.jpg)

The second step is to create application. How to create Application your can read [here](https://developer.artik.cloud/documentation/tutorials/your-first-application.html#create-an-application).

![Illustration of system](/images/Rule.jpg)

**Tracker**

My application for controller uses [Project Tyrus](https://tyrus.java.net/) to create the WebSocket connection.

To configure an application you need to fill these fields with your data. This data you can find [here](https://artik.cloud/my/devices) in the menu device info.

public static final String DEVICE_ID = "<YOUR DEVICE ID>";

public static final String DEVICE_TOKEN = "<YOUR DEVICE TOKEN>";

To start working you need to press the button "Do It" and then to wait for the ARTIK Cloud response (_"Connected" if everything is okay or "Error code" if not_)

![Illustration of system](/images/device-2016-09-26-202827.png)

**Controller**

For the controller I used ARTIK API Java/Android. For my app I used the application from the example. For the configuration you need to config next fields:

_// Copy from the corresponding application in the Developer Dashboard_

public static final String CLIENT_ID = "<YOUR APPLICATION ID>";

_// Copy from the Device Info screen in My ARTIK Cloud_

private final static String DEVICE_ID = "<YOUR DEVICE ID>";

_// Response of application when success authorized_

public static final String REDIRECT_URL = "<REDIRECT URL OF APPLICATION>";

When app starts the first time you need to login in your Samsung account.

![](/images/device-2016-09-26-192830.jpg)

Also ARTIK Cloud has an interface which allows you to monitor messages and actions of your device.

![DataLog](/images/DataLog.jpg)

**Demonstration**

<iframe width="638" height="365" src="https://www.youtube.com/embed/sn_eqTXcs9E" frameborder="0" allowfullscreen></iframe>

**The possible use of the system**

<iframe width="638" height="365" src="https://www.youtube.com/embed/rIoEeqKyUf4" frameborder="0" allowfullscreen></iframe>


[Git source tracker](https://github.com/Lrakulka/ARTIK_Cloud_Camera_Tracker)

[Git source controller](https://github.com/Lrakulka/ARTIK_Cloud_Camera_Controller)

