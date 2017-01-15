---
layout: post
title:  "Extender of Smartphone Camera"
date:   2016-03-14 00:00:00 +0100
categories: modification camera extender
previewImg: http://lrakulka.github.io/images/tmp_image_0.jpg
info: This project allows to take out the camera module of smartphone.
---

![](/images/tmp_image_0.jpg)

**About this project**

This project allows to take out the camera module of smartphone.

**Story**

I am currently working on a big project which requires portable computer and fixing the camera on head. This project is the hardware part of my big project.

I decided to modify a smartphone because it has all what I need for my project except possibility to take the camera module outside. I took my LG P700 for experiment with the camera extending.

I started from collection of information and found next scheme of the main camera connector of LG P700. I used it to create a connector for my adapter.

![](/images/RyZoPjBQ789DGUIaiQdm.jpg)

![](/images/SqMJsLcgIzWHZz6VQ51i.jpg)

I tried to create the adapter twice. The first try was not successful but I found out that my phone camera module connect to motherboard with a 40-pin connector, where 7 pins are used to supply electricity of 1.8 and 2.8 voltage, 10 pins are paired into 5 signal lines for transmitting of high speed digital signals, other pins are used for ground or for low speed signals.

![The connector of camera modul](/images/4r76GBUN4gFwvOBJjmkd.jpg)

Also I found out that I need to put capacitors because extending of the signals line length are decreasing power of signal ( That is why I used capacitors to solve this problem, I placed the capacitors between the ground and the data line. In addition, the capacitors must be placed as close to the camera connector as possible). The second problem that I faced was the different signal lines length which was cause of error in the camera module. I solved this by making all the lines the same length.

![The adapter's connector scheme](/images/bW7VngGVfrKJ4MZUXfH9.jpg)
![](/images/EFNp9h9qppxFWFvjQ09p.jpg)

<iframe width="638" height="365" src="https://www.youtube.com/embed/evcpW-UT2E4" frameborder="0" allowfullscreen></iframe>
