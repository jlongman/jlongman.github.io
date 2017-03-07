---
layout: post
title:  "Chromecasting simple pages"
date:   2017-03-07 18:16:01 -0500
categories: Code Geekery
---
tldr: Casting static webpages works, but not well; I list some challenges, ways to do it, things I'm working on related to it.

So you want to ChromeCast webpages, without using mirroring?  Great idea, it means you can browse away from the page and not use up all that battery on your portable device. Chromecasts are a relatively simple and cheap way to get stuff, media and even pages, from the web onto cheap and large screens.  It also works well in social groups, guests can add and sometimes queue additional content (depending on the content).

For non-playback media like webpages maybe you just want to show a sign, or a website, or the latest news or whatever.  You can't however change font-size, scroll around, click, do anything useful. Chromecasting works best if your webpage shows everything on one page, like a dashboard.

Here's a simple ChromeCast sender/receiver combo that works in desktop Chrome: [chromecast-dashboard](https://boombatower.github.io/chromecast-dashboard/sender/). It will relay the webpage you specify and then you can leave the webpage and continue browsing. You get a pretty good idea of how your page will do on a ChromeCast

One of the problems is the CC is weak. Like really weak. Can barely show (JavaScript/dynamic) Google Maps weak. For casting a [local transit dashboard](https://github.com/jlongman/atlasboard-localdash) I ended up using the Google Maps Static API to draw the maps I needed. (Some of the overhead was definitely the dashboard).  So it's not clear what you can show.

And now there's multiple versions of the ChromeCast, the [1st Generation](https://en.wikipedia.org/wiki/Chromecast#First_generation), 2ns [Generation](https://en.wikipedia.org/wiki/Chromecast#Second_generation), and [Ultra](https://en.wikipedia.org/wiki/Chromecast#Chromecast_Ultra) (And the audio, but it's not relevant to this discussion).  So you should probably check it works on all the hardware you need to use.

Authentication is also an issue. I wanted to show some Google Calendar on the ChromeCast and unless the calendar is public, you're not going to be able to show it without mirroring.  Strangely when viewing the calendar the ChromeCast just went black, whereas my iOS app actually showed the Google login page.

(Well, maybe there's a way to get an auth token in the URL - I haven't looked too deep at that yet. I actually kind of hoped when I was using a ChromeCast sender/receiver from desktop Chrome that it would use the auth from the browser - but it didn't.)

[Dashkiosk](https://dashkiosk.readthedocs.io) is a webpage-casting kiosk solution I found that runs on linux - it's great! - the author says you'll have to test the pages and recommends putting extremely simple pages between complex pages to maximize the chance you don't run out of memory and slowdown/crash the ChromeCast.  Some users even recommend forcing the ChromeCasts to reboot periodically to prevent slowdowns and the rare case when the ChromeCasts stop responding.

Oh, as far as mirroring - desktop Chrome will do it, and on iOS I've used EZCast - no I'm not going to link it - but the experience was awful.  The iPhone screen not being close to 16x9 and scrolling and font-size issues made it painful.

Anyways, I've got some code rumbling for my [Anchors](https://jlongman.github.io/anchors) app that will attempt to ChromeCast bookmarked links, and a couple of more ideas of useful ChromeCast apps that I might publish.