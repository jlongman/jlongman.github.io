---
layout: post
title:  "InfraRed to ChromeCast control from the Pi"
date:   2017-04-06 20:15:01 -0500
categories: Code Geekery ChromeCast Python InfraRed HDMI-CEC LIRC
---

Wherein we put aside questions left unanswered earlier and document the InfraRed receiver controlling the ChromeCast.

# Trailing questions from before

(re: monitoring hdmi-cec) As near as I can tell the ChromeCast doesn't send the "Power on" and "Change input" CEC codes without a television - Device 0 in the CEC bus parlance - connected.  This means we listen for the ChromeCast CEC codes to relay to the blaster to turn on the TV and change the TV's input to the ChromeCast.  I'd even downloaded the master version of `libcec` and `cec-utils` to ensure this wasn't something not available in the older version of `libcec`.

For now I think the `pychromecast` code mentioned [earlier](/2017/Double-More-CC-Pi-Control) is failing because the Raspberry Pi is using an IPV6 address and the python bonjour/zerconf implementation talking to ChromeCast doesn't like that.  It works intermittently, strangely enough, but not enough to be reliable.

# InfraRed (IR) Joy

(Note this is all predicated with the [chromecast-button](https://github.com/jlongman/chromecast-button) work - so `cec-utils` is already installed, see my other posts.)

But in the meantime I've received my InfraRed (IR) receiver (after robotshop sent it to my old apartment and I retrieved it from the gracious new tenants). Hooked up to the Pi, we can use the unused `play`, `pause` and `stop` buttons on our TV remote to do something useful like pause or stop the ChromeCast.  This is about 5 times faster than using your phone to pause the ChromeCast. (!!!)  

**Note that if you incorrectly wire an IR receiver it is _very_ easy to blow it - it will not function at all in this case!  Double check your wiring before applying power!**

There are a lot of older instructions out there, but basically to get the IR Receiver working you need to:

- correctly wire the IR receiver - **while the Pi is off!**
	- I used a TSOP38238, a very standard 3v IR receiver
	- I prefer wire-wrapping with a 32 guage wire, it's quick and clean and easy to undo if not as robust as soldering.  It's also easy to chain the button and the IR blaster and receiver on the power and ground posts. Most use cases like this it's good enough. 	
- edit `/boot/config.txt`, and uncomment

	`dtoverlay=lirc-rpi`
	- since I'm using the default pins (17 and 18) I didn't specify them.
	- reboot to see the lirc devices in `/dev/lirc*`
- install lirc

	`sudo apt-get install lirc`	
	- one should then be able to stop the lirc services `sudo /etc/init.d/lirc stop` and then run `mode2`, press buttons on a remote control to see output on the screen.
- The process to configure lirc is pretty standard so I'll let the reader find these.  I basically ensured the `lirc` service was installed and then configured the codes I wanted to send for my blaster portion and then used `irrecord` to get codes I wanted to be able to read and added them in `/etc/lirc/lircd.conf`.
- BUT I ALSO needed to change the `LIRCD_ARGS` line in `/etc/lirc/hardware.conf` to be

    `LIRCD_ARGS="-o /var/run/lirc/lircd"`
    - only with this edit and once `lircd` was restarted did I then have a valid lircd socket which I could use with `irexec`.  Without this you will see failure messages when doing a `sudo /etc/init.d/lirc status`.  But you must also have a valid config in your other files too!
    - the `/etc/lirc/lircrc` I used uses the https://github.com/jlongman/chromecast-button project's scripts, but these could also just be the proper `cec-client` commands - and indeed maybe the path is incorrect and the `$device` variable is just defaulting to 4 instead of using the correct one.  See below:

<script src="https://gist.github.com/jlongman/084c1c98ee81417b0e9c15811f9ec022.js"></script>

This was good enough to get it going.  I could now press `pause/play` once to pause and again to play.  Note that because the state is in `irexec` (and separately in the `chromecast-button/button.py` script) that mixing the two will de-sync these states.  

Solving this would require the state to be isolated and shared - possibly in the shell script - or being able to read the current ChromeCast status (see above about pychromecast/IPV6).

Users _love_this system even more than using the [button](/2017/More-CC-Pi-Control/).  It's sit back and pause (and later resume) means you know if there's a problem by the time you could walk to the button or even unlock your phone.  Mission accomplished

# Further improvements

At this state the project works and moves to the backburner.  It's functional and got me through my illness when I was too feverish to code.  However...

More work can be done to understand the `pychromecast` problem.  Being able to talk to the ChromeCast might mean adding the ability to start a default an app or app with video or playlist when nothing is already playing, when an input occurs.

The [python-lirc](https://github.com/tompreston/python-lirc) module could be integrated with the [chromecast-button](https://github.com/jlongman/chromecast-button) code so the state is unified.  The button could alternatively be used to trigger IR blasting.

The [python bindings of libcec](https://github.com/Pulse-Eight/libcec/tree/master/src/pyCecClient) could also be used to eliminate the cec-client binary - and indeed if other CEC sources were transmitting we could use this binding to wait for CEC events and trigger IR blasting.

And there's always more to do.