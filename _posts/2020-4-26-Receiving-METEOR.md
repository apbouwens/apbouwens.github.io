---
layout: post
title: Receiving METEOR weather satellite images
mathjax: false
comments: true
categories: [Weather satellite, METEOR, RTL-SDR]
---

In the [previous post]({% post_url 2020-4-19-Receiving-NOAA-start %}) I talked about how I was mesmerized by the beautiful images radioed down to earth by the NOAA satellites. These use the analog [APT system](https://en.wikipedia.org/wiki/Automatic_picture_transmission), which dates back to the 1960s. So clearly, there should be room for (digital) improvement.

And indeed there is, but there's just one lonely satellite sending down higher resolution, digital images in the easy 137 MHz range: the Russian [Meteor-M N2](https://directory.eoportal.org/web/eoportal/satellite-missions/m/meteor-m-2). Decoding these images is quite a bit more complex than for the APT images, but you do get rewarded for your trouble:

![METEOR weather satellite image]({{ site.baseurl }}/images/meteor_first_receive.jpg)

<!-- more -->

## Low-rate picture transmission (LRPT)

Meteor-M N2 uses the [low-rate picture transmission (LRPT)](https://en.wikipedia.org/wiki/Low-rate_picture_transmission) protocol to beam down images digitally. According to the Wikipedia page, LRPT has a resolution of 1 km/pixel, compared to 4 km/pixel for APT.

There's also the [High-resolution picture transmission (HRPT)](https://en.wikipedia.org/wiki/High-resolution_picture_transmission), which can be [received from NOAA and other satellites](https://tysonpower.de/blog/hrpt-optimizations-and-great-images), but it is broadcast in the L-band at 1.670–1.710 GHz. Hence, it requires more complicated and expensive equipment such as a tracking dish antenna. Hope to get there eventually.

## V-dipole antenna

I used the same set-up to use APT transmission from NOAA satellites: a V-dipole mounted on a broomstick connected directly to an RTL-SDR v3, see  my [previous post for details]({% post_url 2020-4-19-Receiving-NOAA-start %}).

## Demodulating the LRPT signal

To receive APT images, it is enough to record the audio signal in SDR# and then process the audio file in one of the many decoders. This doesn't work for the LRPT signal. Here, we need to record the full baseband signal, centered at 137.1 MHz. The large sample rate (1 MS/sec, 8 bit per sample) leads to very large files very quickly (roughly 1GB in 10 minutes).

Instead, I used the Meteor Demodulator plugin for SDR#, [found here on happysat.nl](http://happysat.nl/meteor_2.3.zip). The plug-in will demodulate the signal, resulting in much smaller files. It also gives some immediate feedback telling you whether the signal is useable. See the screenshot below, with the plug-in active on the left.

I tuned the RTL-SDR to 137.1 MHz. There's no need to adjust for the Doppler shift, because the demodulator will track the correct frequency. The first few passes of the METEOR satellite where too low to receive any useable signal, but a few attempts later I did get a decent signal:

![SDR# screenshot of METEOR reception]({{ site.baseurl }}/images/METEOR_SDR_screenshot.jpg)

When the signal is strong enough, the demodulator plug-in shows "Locked" in red text next to the frequency, indicating that a lock on the carrier frequency of the QPSK signal. In the constellation plot below, you can start to see four separate point clouds, indicating that SNR is high enough to discriminate between the four symbols of the QPSK signal. If the signal isn't strong enough, you only get one big hazy point cloud.

The demodulator writes the demodulated symbols to a '.s' file. To convert the .s file into an image, I used the LRPTofflineDecoder from the [Meteor LRPT Suite](https://leshamilton.co.uk/MeteorLRPTSuite.htm) by Les Hamilton. CLick the '72K' button to load the '.s' file. The '72K' stands for the 72k symbol rate used by METEOR-M2.

![SDR# screenshot of LRPT decoder]({{ site.baseurl }}/images/LRPTdecoder_screenshot.jpg)

## Decoding the image

Decoding the '.s' file yields three greyscale images, which the decoder indicates as '0.5-0.7', '0.7-1.1' and '1.6-1.8'. I'm assuming these are the spectral bands with the wavelength in micrometers. I created an RGB image by setting the R, G and B channels each to one of the spectral bands:

![METEOR RGB image]({{ site.baseurl }}/images/meteor_rgb.jpg)

I couldn't believe how easy it was to get this image! I read that receiving METEOR was quite a bit more complex than APT, but thanks to all the great tools out there, I found it quite easy. I didn't expect to get good signals for APT in the first place, since I live within about 500 m of an FM broadcast station in the center of a city (Brussels).

Besides the awesome resolution, there are two things that are striking about this image.

- The colors aren't natural (blue snow)! That was to be expected since I set the blue channel to contain the infrared image (1.6 µm - 1.8 µm). To get natural-looking color, the spectral bands should have followed the [spectral sensitivity of the human eye](https://en.wikipedia.org/wiki/CIE_1931_color_space). But of course, this being a weather satellite, the infrared (temperature-related) spectra are much more interesting.

- The image seems compressed inwards near the left and right edges. I guess this is how the METEOR's lens deforms the image to get more land area onto its sensor.

## The spectral bands

Here are the three different bands shown separately:

| 0.5 µm - 0.7µm | 0.7 µm - 1.1 µm | 1.6 µm - 1.8 µm |
|----------------------------------------------------|
| ![METEOR R image]({{ site.baseurl }}/images/meteor_blue.jpg) | ![METEOR G image]({{ site.baseurl }}/images/meteor_green.jpg) | ![METEOR B image]({{ site.baseurl }}/images/meteor_red.jpg) |

The contrast for snow on the Alps and in Norway is very striking in the visible light (0.5 µm - 0.7µm) image, while it appears dark (cold!) in the IR image (1.6 µm - 1.8 µm).

## A more natural-looking image

To create a more natural-looking image and undo the stretching, I used the [SmoothMeteor software](https://leshamilton.co.uk/meteor3m.htm), also by Les Hamilton from whom I downloaded the Meteor LRPT Suite.

I chose the "Green land" color scheme to get the picture at the top if this page:

![Stretching the METEOR image with smooth Meteor]({{ site.baseurl }}/images/meteor_stretch.jpg)

## Future

In the future, I hope to get less missing lines in the pictures by using an FM bandstop filter and LNA at the antenna. Let's see how much that will help.

Meteor-M N2 is the only sat emitting LRPT in the easy VHF band for now... Two other Meteors were launched, but the first one, [Meteor-M N2-1](https://en.wikipedia.org/wiki/Meteor-M_No.2-1) was lost during a launch failure, and the second one, [Meteor-M N2-2](https://directory.eoportal.org/web/eoportal/satellite-missions/m/meteor-m2-2), worked for some time, but seems to have been hit by a micrometeorite and isn't broadcasting on VHF anymore.

I really hope the [Meteor-M N2-3](https://www.wmo-sat.info/oscar/satellites/view/615) launch will happen this year (2020) as planned!

This was a rainy day in Belgium:
![METEOR R image]({{ site.baseurl }}/images/METEOR_rainyday.jpg)
