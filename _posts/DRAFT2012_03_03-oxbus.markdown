---
layout: post
title: Oxbus: bus times in Oxfordshire
date: 2012-03-02T08:00:00.1Z
---

Last Wednesday, I spoke at [Oxford Geek Nights #25](http://oxford.geeknights.net/2012/feb-29th/) about [Oxbus: a mobile bus times site for Oxfordshire](http://www.oxbus.co.uk). Here's why and how I built it.

You can get live bus times for Oxfordshire at [http://www.oxontime.com](http://www.oxontime.com). I think the fact that it is possible to get these times is fantastic, but I think it's really disappointing that the user experience on that site is so poor. It should be easy to get times for nearby stops.

So I decided to make it easier, by building a better site.

### Getting the stop times ###
Unfortunately, there's no easy way to get the data on bus times - there isn't an API or anything like that. So I wrote a Python module to scrape the consumer facing site: [BusScraper on PyPI](http://pypi.python.org/pypi/BusScraper/), [BusScraper on GitHub](https://github.com/matthewowen/busscraper). In a rare moment of fortune, the same system is used on a number of sites, which means this module works in Bristol, South Yorkshire, Kent etc as well as Oxfordshire. It does a few things, but the important thing is that if you give it a stop ID, it will give you the buses departing from that stop in the next hour.

This isn't very efficient and it is pretty brittle, but it is the only route available at the moment.

### Getting nearby stops ###
To figure out the nearby stops (so we can pass their IDs to the scraper), we have to do two things: figure out where the user is, and then figure out which stops are near that location. Thankfully, both are pretty simple to do. 

To get a user's location, we can just use [geo.js](http://code.google.com/p/geo-location-javascript/). It is delightfully simple, and takes the pain out of cross browser / platform location. use the getCurrentPosition method and it will try to do so, run different functions if it can or cannot find location, and make the user's lat/long available.

Wonderfully, it is possible to get [NapTAN data for the entire UK](http://data.gov.uk/dataset/naptan); this is all the points of access to public transport. I took a subset of this (stops in Oxfordshire), trimmed it to the fields I need, and put it in an SQLite database. SQLite isn't fashionable, but it is useful; as the application only ever reads from the database, it is very useful to be able to easily version control the database. To figure out distance and proximity, I'm just doing basic pythagorean trigonometry - the distances are so short that it isn't worth considering curvature, and at this long/lat an unsophisticated conversion of lat/long units to metres is sufficient for the purpose of this app.

### Bringing it together ###
So, you've visited [http://www.oxbus.co.uk](http://www.oxbus.co.uk). It has found your location. It has forwarded you to a URL with your location in the path, and found the ten nearest stops. For each of those stops, the application will use BusScraper to find the next buses. It'll then store that data in a redis cache for 60 seconds (any longer and the data stops being useful), to help make heavy usage a little more efficient. It'll then display them in a list. Javascript is used to show/hide the later stops - the aim being to minimise the amount of (potentially slow) requests you need to make to the server. If you want to see stops that are further away, you can scroll down to the bottom of the page and tap to show more. This'll fire an AJAX request to add in the next ten stops.

### Mapping ###
I started simple. But I wanted to build it out more. I started by smartening things up, but the big step was introducing maps. By tapping 'show on map', you can see the stops location on a map, and see directions from you to the stop. This makes it a little easier if you're out and about in an area that you aren't familiar with.