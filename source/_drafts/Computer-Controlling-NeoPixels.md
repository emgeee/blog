---
title: "Computer Controlling NeoPixels"
layout: "post"
permalink: "/Computer-Controlling-NeoPixels.html"
uuid: "3821298751883428975"
guid: "tag:blogger.com,1999:blog-74857919386821651.post-3821298751883428975"
date: "2014-05-13 02:20:00"
updated: "2014-05-13 02:20:35"
description: 
blogger:
    siteid: "74857919386821651"
    postid: "3821298751883428975"
    comments: "0"
categories: [electronics, arduino, alarm clock, hardware]
author: 
    name: "Matt Green"
    url: "https://plus.google.com/114242571581942593905?rel=author"
    image: "//lh5.googleusercontent.com/-XNtg5NpWxVA/AAAAAAAAAAI/AAAAAAAAAMo/P-xg9PWMok4/s512-c/photo.jpg"
published: "false"
---

The other day I received a [NeoPixel](http://www.adafruit.com/category/168) strip from [adafruit.com](http://adafruit.com./). NeoPixels are really cool tricolor LED lights that are individual addressable and come in several form factors including strips and, rights, and matrices. As is my habit whenever I buy new electronic gadgets, my first order of business was to wire the strip to be controllable by a computer. Doing this first entails hooking up the light strip to a power source and connecting it to and Arduino, which will ultimately serve as the intermediary between the computer and the light strip.

(http://1.bp.blogspot.com/-vB0HrbAX28U/U3F8hiIoNCI/AAAAAAAAAWo/575ABhunpl0/s1600/IMG_20140512_185127.jpg)(http://1.bp.blogspot.com/-vB0HrbAX28U/U3F8hiIoNCI/AAAAAAAAAWo/575ABhunpl0/s1600/IMG_20140512_185127.jpg)

Using the wonderfully easy [NeoPixel Arduino library](https://github.com/adafruit/Adafruit_NeoPixel) with some of the provided sample code, I was able to confirm everything was wired correctly while also loosing focus temporarily due to the pretty flashing lights...

Back on track.

nitially, I wanted to use the the [firmata](http://www.firmata.org/wiki/Main_Page) Arduino library to facilitate the computer-&gt;NeoPixel communication, however, I quickly realized that this would not work. Firmata was designed to be able to give the computer control over the Arduino's individual I/O pins and resources including things like PWM and I2c. This means that if you want to diddle a pin several hundred times a second, you would need to have the the host computer send an individual command over USB to the Arduino to either set the pin high or set the pin low. Timing was already critical with the NeoPixel protocol and having that extra latency was just not going to work. Instead, I settled on a a very simple variable byte protocol that allows me to easily send a command to control the entire strip as one unit or toggle individual pixels as necessary. Essentially, the Arduino sits and waits for a single byte to be received over serial and depending what that byte is, will either perform an action or wait for more bytes. Currently, this solution has a limit of 252 different NeoPixels that can be controlled, by seeing as how I only have 60 hooked up, this shouldn't be a problem.

The light strip can now be controlled via language that can spit bytes out over a serial port but my go-to language for these situations tends to by Python. Armed with the trust [pySerial](http://pyserial.sourceforge.net/) library, I can now add a fancy light show to any Python program I wish.

This is only the beginning of this project, there is more to come!
