---
layout: post
title: Using Facebook's Graph API on Ubuntu 10.04? Try disabling IPv6
date: 2012-10-14T21:00:00.1Z
---

I've been working on [Memetracer](http://www.memetracer.com/), a way of visualising the posting and popularity of links across the social web. Obviously, it uses various social media APIs. Everything was hunky-dory on my local machine (Ubuntu 12.04), but I ran into some problems on my server (Ubuntu 10.04).

Calls to my application's own API (which pulls together information from the different social APIs) were taking an intolerable amount of time to return. Working through the different possible points of problem, I managed to isolate it down to the call to Facebook's [Graph API](https://developers.facebook.com/docs/reference/api/) - remove it, and everything works just fine.

Testing further, I found that cURL-ing any requests to http://graph.facebook.com took an age to resolve. Adding a '-v' flag revealed the problem. Two attempts to resolve to an IPv6 address were taking far too long to fail, before falling back to an IPv4 address. Thankfully, a quick trip to /etc/sysctl.conf lets us disable IPv6, and now everything is as quick as I'd hope - IPv6 problems on 10.04 are apparently 'a thing'

It's not a great solution - IPv6 is the future, after all. But until I get round to updating to 12.04, it'll do.