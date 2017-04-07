---
layout: post
title:  "Controlling the ChromeCast from the Pi"
date:   2017-03-07 18:16:01 -0500
categories: Code Geekery
---
tldr: I want my Pi to control my ChromeCast, with either HDMI-CEC or the ChromeCast API

I have a TV that does not support [HDMI-CEC](https://en.wikipedia.org/wiki/Consumer_Electronics_Control).  I have a Chromecast which is annoying to pause quickly, especially when the phone which started the video forgets about the ChromeCast (this happens less than it used to at least).  I want to use the Pi to pause the Chromecast.

The easy part is the button and/or IR receiver on the Pi - hook that up and trigger a script.

Alternative A:

What I thought was the quick and dirty solution was to use `libcec` or `cec-client` on a Raspberry Pi.  Send the [magic numero](http://www.cec-o-matic.com) and pause the connected ChromeCast.  This avoids talking to the network, it's always clear which CC is to be controlled etc.

But so far it doesn't work.  I can see it.  It has a strange address (`f.f.f.f` == invalid).  But when I try to send it messages no response.  I added an appendix with an example command er... appended.

Alternative B:

Use the [CC API](https://developers.google.com/cast/) to control the CC, which is fine and I've used it on iOS before, but I'll need to make sure it's controlling the correct CC.  I'm not sure how on the Pi, but I'll find out.

SO I [posted](https://www.reddit.com/r/Chromecast/comments/5znpuk/i_want_to_use_a_raspberry_pi_to_control_the/) this all on reddit, including:  Does anyone have success controlling a CC using `libcec`?  And could someone put the Pi into monitor mode (`cec-client -m`) and use their remote control on the TV to pause the CC?  It'll be curious to see what the response is.

The end-product would be open source if it helps motivate anyone.

Appendix for Alternative A:
This is what I see

     pi@raspberrypi:~/libcec/build $ echo "scan" | cec-client -s  -d 1
     opening a connection to the CEC adapter...
     requesting CEC bus information ...
     ERROR:   [           12174]	failed to request the physical address
     CEC bus information
     ===================
     device #1: Recorder 1
     address:       2.0.0.0
     active source: no
     vendor:        Pulse Eight
     osd string:    CECTester
     CEC version:   1.4
     power status:  on
     language:      eng
    
    
     device #4: Playback 1
     address:       f.f.f.f
     active source: no
     vendor:        Unknown
     osd string:    Chromecast
     CEC version:   1.4
     power status:  on
     language:      ???
    
    
     currently active source: unknown (-1)
     pi@raspberrypi:~/libcec/build $

also tried these which should variously pause and play:

     echo "tx 14 44 46" | cec-client -s -d 1
     echo "tx 14 44 61" | cec-client -s -d 1
     echo "tx 14 44 45" | cec-client -s -d 1
