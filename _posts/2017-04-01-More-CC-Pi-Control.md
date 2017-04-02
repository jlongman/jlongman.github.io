---
layout: post
title:  "More ChromeCast control from the Pi"
date:   2017-04-01 20:59:01 -0500
categories: Code Geekery
---

Already useful!  This article details Button to ChromeCast, and simple IR blasting.  A later article will talk about more complex IR blasting that integrates the ChromeCast.

So I've been really sick recently, and while I wasn't able to code (except in fever dreams) for the longest time I could wire electronics and do basic sysadmin tasks.

The result is a Pi with a button that pauses the attached ChromeCast using HDMI-CEC even if your TV doesn't support HDMI-CEC. And especially if it doesn't!  Which is way more useful.

(HDMI-CEC is a bus protocol that allows control information to be shared by all devices attached in an HDMI network, via some form of HDMI switch, like a TV having multiple HDMI inputs.  So the play button on your TV remote can talk to your DVD player or your player can tell the TV to make it the active source when a disk is inserted and play starts)

# The Button

The button is a simple interface that lets you control the system in a direct and local manner.  Great for pausing the ChromeCast as you're walking out to the balcony.

- Get a Pi! I had an old Raspberry Pi v.1 B that was perfect 
- Install raspbian lite
- Install `cec-client` - `sudo apt-get install cec-utils`.
- Attach a button to GPIO 4
 - I had a button module that I dremeled into the Pi case I had.
- Get this code [https://github.com/jlongman/chromecast-button](https://github.com/jlongman/chromecast-button)
    - executes `pause.sh` or `play.sh` based on last input - making `cec-client` calls
	- Modify the pin used in `button.py `if necessary
- `sudo crontab -e` - start the button script at boot
	- add `@reboot python /home/pi/chromecast-button/button.py &`
- Test pause and play the ChromeCast by pressing it in successive times.  It can take a second or two to take effect.

## Troubleshooting

If the ChromeCast isn't pausing/playing then use `echo scan | cec-client -s -d 1` to scan the attached HDMI devices seen by the Raspberry Pi.  You will need to modify `chromecast/play.sh` and `chromecast/pause.sh` to the appropriate device e.g. change `echo 14:44:etc` to `echo 16:44:etc` if the ChromeCast is device 6 (note I didn't verify if this is a valid Playback device).

# Basic IR blasting

I had an IR receiver get redirected to an old apartment so while I'm waiting to recover my parts I found a valid `lircd.conf` file for my partner's 'TV and even the cable-box/PVR.

The current solution for the blaster is a jack dremeled in the case and IR cables I ordered from China.  Right now I'm driving it directly off the default lirc blaster pin without any limiting resistor or transistors.  My thought is because IR signals are transient maybe it won't be so bad.  I also ordered 10 of the cables so if I burn this one out I've got time to re-order and re-design before I run out of spares.

The TV config file wasn't complete so I needed to add some codes - see [here](https://gist.github.com/jlongman/a7a56241506681083d9b939828716dd6).  Visual inspection of the codes revealed some repeating patterns - specifically the 2nd and fourth bytes, so I [generated](https://gist.github.com/jlongman/539da04afeeae41c108cd9962c9d7b32) a `lircd.conf` with all possible hex values.  This made it easy to test a value by going `irsend SEND_ONCE seiki KEY_2DDF` to test the 0x2ddf value.

<script src="https://gist.github.com/jlongman/539da04afeeae41c108cd9962c9d7b32.js"></script>

Eventually I found the Input Source menu, and then that the `KEY_TV` button would take the input to the USB key.  When you don't have individually addressable inputs you need absolute positioning.  This allows me to go the USB interface, wait, then scroll up a known amount to get to any specific input.

- install lirc `sudo apt-get install lirc`
- edit `/boot/config.txt` to enable the `rpi-lirc` overlay
    - you may need to change the pins your blaster (and possibly receiver) are attached to.
    - attach your blaster
- copy in your appropriate `lircd.conf` file
- restart lirc - `sudo /etc/init.d/lircd restart` or reboot
- test your lirc - `irsend SEND_ONCE seiki KEY_POWER`
  - `seiki` will match your remote name in the `lircd.conf` file
  
  This is an example of a script that changes the input by going to the USB device - a known position - and then using buttons to go to a specific input.
  <script src="https://gist.github.com/jlongman/4ed0e6d0e755b10df3afd674656021ec.js"></script>
  
# Remaining work
 
The SD card died on my Pi so I'm rebuilding it now - at least it made this blogging task easy - but the next steps are basically to monitor the HDMI-CEC bus and respond with IR codes to turn on the TV and change to the ChromeCast's input. 
 
Once the IR Receiver is finally in my hands then I can use the TV's remote's pause button to pause the ChromeCast.

I'd also like to add code to detect when the ChromeCast is not already active so that the button could _start_ a video when the button is clicked and nothing is happening.  Say, a yule log near Christmas or that poor pregnant giraffe my partner is stalking (go [April](http://www.aprilthegiraffe.com)!).
 
