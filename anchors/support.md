---
layout: page
title: Support
status: draft
type: post
published: true
comments: true
---

# Frequently Asked Questions

If you don't find the answers below, jump to [email](#mailto) support.

## What is an anchor?

An anchor is an element in a web page which can be browsed and scrolled to directly when correctly referred to. 
This used to refer to the specific HTML anchor (i.e. `a`) tag but now includes `id` elements within pages.
The **Anchors** app additionally has support for HTML-style headers and Wikipedia-style headers.

## Why is my **Anchors** app empty?

Links are added to the **Anchors** app from MobileSafari or other web browsing tools using the **AnchorsUp** app extension.

## How do I access the **AnchorsUp** app extension?

1. Ensure that you have installed the **Anchors** app.
2. Go to MobileSafari and a web page, then hit the Share button along the button menu item of the browser.
![The AnchorsUp App Extension]({{ site.url }}/assets/img/anchors/share-button-safari.png)

3. Scroll to the far left of the app extension icons - the bottom row with grey icons - and select the More icon with three dots.
<br />![The AnchorsUp App Extension]({{ site.url }}/assets/img/anchors/more-sharing-button.png)

4. In the table of Activities, find the **AnchorsUp** app extension which was installed with the **Anchors** app.
<br />![The AnchorsUp App Extension]({{ site.url }}/assets/img/anchors/anchorsup-disabled.png)

5. Enable the **AnchorsUp** app extension by turning it on such that the button shows green.  Optionally order it so that it appears first so you don't need to scroll around everytime to find it.
![The AnchorsUp App Extension]({{ site.url }}/assets/img/anchors/anchorsup-enabled.png)

7. Click Done and then find the **AnchorsUp** extension in the Activities.
![The AnchorsUp App Extension]({{ site.url }}/assets/img/anchors/anchorsup-extension-button.png)
 
8. Click to open it.

## I clicked on an anchor, now what?

If the anchor was on the current page, the page has now been scrolled to that anchor.
If the anchor is on a different page, the app does not change pages, but the link is still saved.
The web URL has also been copied to your clipboard so you can paste it in an email or chat.
The anchor has also been added to the **Anchors** app as the most recent item and can be shared from there later.


The **Anchors** app keeps a history of the number of times you've shared 
the link through the Anchors app, as well as when you first browsed there, and when you
last shared it. It also shows a preview of the anchor.

## Why!?

I was arguing on the internet and wanted to make a point using Wikipedia.  There was an entire section dedicated to what I was trying to prove and I couldn't get iOS to show me the link - something trivial on a desktop using Inspect Element and other tools.  So I scratched that itch with this.  Also I learned Swift and iOS development doing this.

# Email support form



<form id="mailto" action="mailto://jl.anchors@gmail.com" method="post" enctype="text/plain" >
Email:<input type="text" name="Email"/><br/>
Website:<textarea name="Website" cols="65" rows="2"/><br/>
Problem:<textarea name="Problem" cols="65" rows="10"/><br/>
Fill this out and send from your mail client after you click Submit.

<input type="submit" name="submit" value="Submit">

</form>
