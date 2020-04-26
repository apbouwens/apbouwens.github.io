---
layout: post
title: Receiving METEOR weather satellite images
mathjax: false
comments: true
categories: [Weather satellite, METEOR, RTL-SDR]
---

In the [previous post]({% post_url 2020-4-19-Receiving-NOAA-start %}) I talked about how I was mesmerized by the beautiful images radioed down to earth by the NOAA satellites. These use the analog [APT system](https://en.wikipedia.org/wiki/Automatic_picture_transmission), which dates back to the 1960s. So clearly, there should be room for (digital) improvement.

And indeed there is, but there's just one lonely satellite sending down higher resolution, digital images in the easy 137 MHz range: the Russian Meteor M-2.

![METEOR weather satellite image]({{ site.baseurl }}/images/meteor_first_receive.jpg)

<!-- more -->

Meteor M2 uses the [low-rate picture transmission (LRPT)](https://en.wikipedia.org/wiki/Low-rate_picture_transmission) protocol to beam down images digitally. 

There's also the [High-resolution picture transmission (HRPT)](https://en.wikipedia.org/wiki/High-resolution_picture_transmission), which can be [received from NOAA and other satellites](https://tysonpower.de/blog/hrpt-optimizations-and-great-images), but it is broadcast in the L-band at 1.670–1.710 GHz. Hence, it requires more complicated and expensive equipment such as a tracking dish antenna. Hope to get there eventually.