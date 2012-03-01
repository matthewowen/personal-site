---
layout: post
title: Principles of Mobile
date: 2012-03-02T08:00:00.1Z
---

Last Wednesday, I spoke at [Oxford Geek Nights #25](http://oxford.geeknights.net/2012/feb-29th/) about [Oxbus: a mobile bus times site for Oxfordshire](http://www.oxbus.co.uk).

You can get live bus times for Oxfordshire at [http://www.oxontime.com](http://www.oxontime.com). I think the fact that it is possible to get these times is fantastic, but I think it's really disappointing that the user experience on that site is so poor. It should be easy to get times for nearby stops.

So I decided to make it easier, by building a better site. In the process, I thought a lot about principles behind a good mobile experience. I don't think [Oxbus](http://www.oxbus.co.uk) is perfect by any means, but it does embody some of the rules I'm talking about.

### Layout ###
With mobile, you are going to have to deal with a range of device widths, font sizes, etc. So forget about fixing widths. I used a minimum width for the site of 320px. Beyond that, any dimensions which are relevant to layout (the relative sizes of areas of the page) are done using %, any that are relevant to text size use ems. This probably limits your ability to use background images, too. Sorry.

In short, if you want pixel perfect, make an app or something. If you're doing a mobile site and you don't want to spend your entire life dealing with trivialities, set yourself some key principles and make it work. Don't sweat the fine detail - it isn't going to work.

### Touch areas ###
Nothing is worse than struggling to tap a link on a phone. Make links big. Make them obvious - buttons. And make the link bigger than the button, for people with clumsy thumbs. And, obviously, don't put small buttons close together, unless you like making your users click the wrong link.

### Minimise page weight ###
The idea that everyone has 3G signals and therefore page weight doesn't matter is nonsense and balderdash. There are lots of times when there simply isn't a 3G signal available. If you want your mobile site to be useful everywhere, you have to minimise page weight. Further to that, ping times for mobile can be truly terrible, so minimise not just the size of requests but the *number* of requests, too.

For Oxbus, I've resisted the attention to include things like jQuery. It would have made my AJAX calls easier, but it's an addition to page weight that doesn't offer enough benefit. I am using Google Analytics, and am inclined to try to remove it - for the simple reason that for an unprimed cache Google Analytics is about 40% of page weight.

### Don't make me type (or touch) ###
Steve Krug's usability advice is that the user shouldn't have to do hard mental work to understand and use your site. On mobile, this still applies, but there's a further principle - don't make them type, ever, unless you can't avoid it. Typing on mobile devices sucks. So does navigating and clicking links. Reading is basically the only thing that doesn't suck. So give people more to read and less to type.

If you visit Oxbus and have given it permission to access your location, you don't have to do *anything*. It just gives you the relevant information. Of course, this is easier for some applications than for others, but I think it is an extensible principle. If you go to Oxbus and want to search by postcode, you can still do so - but you have to intervene in an otherwise automated process. I think it's better to give 90% of your users a totally frictionless process and have the other 10% have to intervene in this process than it is to make people tap about through a billion screens and type in lenghthy essays about their transportation plans for the day.

Make it feel like the future. Figure things out for the user. It'll be nice.

### Prioritise, prioritise, prioritise ###
Obviously you have to prioritise if you want to automate the user's journey down a likely path. But beyond that, you need to prioritise the information you're giving people.

My sense of it is that users are happy to scroll on mobile devices if they know what they're scrolling to (particularly since inertia scrolling is so nice). So the immediately visible area of your device should make it absolutely clear that there's more information if you scroll down.

But, at the same time, we need to give a really strong information scent. Navigating is painful enough that you don't want people to go down blind alleys. The two aims - giving enough information and showing a selection of items - are somewhat orthogonal. There isn't an easy answer to this. 

For Oxbus, I decided that what you really need to know for a stop is:

* What is the stop called?
* How far away is it?
* Where is the next bus going?
* How soon is it

Once you've got that information, you've probably got a pretty good idea of whether it's worth seeing the later stops or looking at where it is on the map. It isn't perfect, but I think it's the right balance in this instance.

### Do it ###
Mobile is an incredible opportunity. The coming ubiquity of smartphones offers an opportunity for a whole new class of service. When people can get information onto their hand held computers wherever they are, whenever, the kind of information that is valuable to distribute over the internet changes enormously. If you can treat mobile as a new platform with its own challenges, you can produce really novel and useful services.