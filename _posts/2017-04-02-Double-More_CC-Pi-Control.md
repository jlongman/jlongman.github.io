---
layout: post
title:  "Double More ChromeCast control from the Pi"
date:   2017-04-02 00:15:01 -0500
categories: Code Geekery ChromeCast Python Pi
---

So naturally I can't let go and kept going on ChromeCast work.  I've since added and pushed the ability to detect the ChromeCast's HDMI-CEC address so this should make the system more reliable in systems with DVD players and amps and so on.

I was also curious about the ability to detect something being played, and if not, then something well known.  With [pychromecast](https://github.com/balloob/pychromecast) this isn't hard - but the order of operations is important:

- install the dev version of python `sudo apt-get install python-dev`
  - I went with 2.7 for now, but 3 is fine.
- install pip `sudo apt-get install python-pip`
- install pychromecast `sudo pip install pychromecast`

If you don't do it in order - especially installing `python-dev` you end up in a weird state where dependencies are not properly installed.  You end up having to find the failed dependency by intuition, almost.  Painful.  (It was even on my last SD card because it was going bad - files ended up being filled with or starting with a series of nulls).

Once `pychromecast` is installed you can query the status of the ChromeCast over the network and make further business decisions.

Doot doot doo: so, it looks like the `YouTubeController` is [unstable](https://www.bountysource.com/issues/27672511-youtubecontroller-doesn-t-display-video) (near the end) - Google keeps changing things and it's obviously an internal API. We can at least monitor whether a ChromeCast is busy, and maybe other apps will work and be more stable.

There still remains further work for [libcec](https://github.com/Pulse-Eight/libcec), like listening for the ChromeCast signalling via CEC.  We can now turn on the TV and switch to the ChromeCast's input when it asks us to - if we can.  It's possible that since my TV doesn't support it the ChromeCast doesn't send those signals - but we'll see when we get there.