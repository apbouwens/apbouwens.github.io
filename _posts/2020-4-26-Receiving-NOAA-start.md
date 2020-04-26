---
layout: post
title: Receiving NOAA weather satellite images
mathjax: false
comments: true
---

When I learned that it is [possible to receive images directly from weather satellites](https://www.youtube.com/watch?v=cjClTnZ4Xh4) using a software defined radio dongle, I knew that was something I had to try one day. 

I didn't get to start trying it out myself until recently (Spring 2020), when I suddenly found myself with quite a bit of spare time. 

I was very surprised to find that it isn't that difficult and can be done cheaply. Here's what I got after a weekend of fiddling around:

![NOAA-15 weather satellite image]({{ site.baseurl }}/images/NOAA15.png 'Image from NOAA-15 in April 2020')

<!-- more -->

There's some noise, sure, but seeing this image come out of the very basic set-up I'll describe here was super exciting.

There are a lot of tutorials out there about receiving images from the American NOAA satellites. I followed [this one](https://www.rtl-sdr.com/rtl-sdr-tutorial-receiving-noaa-weather-satellite-images/) for the most part.

## Software defined radio

I ordered an [RTL-SDR USB dongle](https://www.rtl-sdr.com/buy-rtl-sdr-dvb-t-dongles/) with basic antenna kit and started experimenting.

![RTL-SDR USB dongle]({{ site.baseurl }}/images/rtlsdr_antennakit.png 'RTL-SDR USB dongle'){: width="300px"}

Installation was easy, I just followed [the quick start guide on the rtl-sdr.com](https://www.rtl-sdr.com/rtl-sdr-quick-start-guide/). I used the free software SDR#, which you can [download here](https://airspy.com/download/).

## The antenna

It's important to know that the NOAA satellites emit circularly polarized radio waves at about 137 MHz. Three satellites are still in operation right now:

| Satellite | Frequency  |
|------------------------|
| NOAA 15 | 137.6200 MHz | 
| NOAA 18 | 137.9125 MHz | 
| NOAA 19 | 137.1000 MHz | 

There are a few options for the antenna configuration, and it seems the quadrifilar helix (QFH) is quite popular. Having no access to tools to build one right now, I opted for the horizontal V-dipole configuration, which is detailed [here](https://www.rtl-sdr.com/simple-noaameteor-weather-satellite-antenna-137-mhz-v-dipole/).

I don't really understand why the QFH gets so much attention, as it seems much easier to make the V-dipole using the dipole from the kit. I simply measured the angles such that the two telescopic parts make an angle of 120 degrees, and used a ruler to set their length to approximately 53.4 cm. 

What's also very useful about the V-dipole is that it will strongly attenuate FM radio broadcasts, which use vertically polarized radio waves. This was essential for me, as I live in the center of a city (Brussels) and within 500 meters of an FM broadcast tower. According to [this blog post](https://www.rtl-sdr.com/simple-noaameteor-weather-satellite-antenna-137-mhz-v-dipole/), the V-dipole will attenuate vertically polarized signals by 20 dB, while the QFH only attenuates them by 3 dB.

More details including the radiation pattern of the V-dipole are given [here](https://www.qsl.net/kk4obi/Center-fed%20V-dipoles%20Lateral.html).

I mounted the antenna on a broomstick using the flexible camera tripod which comes with the kit, and placed it on the roof outside my window:

![V-dipole on broom stick]({{ site.baseurl }}/images/NOAA_antenna.JPG){: width="200px"}

## Finding the satellite
 
The NOAA weather satellites follow a polar orbit around the earth, so they can only be received  when passing overhead. To find out when that happens, I used [this page, hosted by AMSAT](http://amsat.org.ar/pass.htm#) (click the green button next to "Weather Sats" on the right).

![AMSAT satellite tracker website]({{ site.baseurl }}/images/amsat.png){: width="400px"}

## Configuring SDR#



