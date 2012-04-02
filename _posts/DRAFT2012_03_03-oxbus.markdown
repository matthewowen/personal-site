---
layout: post
title: Oxbus: bus times in Oxfordshire
date: 2012-03-10T08:00:00.1Z
---

At the end of February, I spoke at [Oxford Geek Nights #25](http://oxford.geeknights.net/2012/feb-29th/) about [Oxbus: a mobile bus times site for Oxfordshire](http://www.oxbus.co.uk). Here's why and how I built it.

You can get live bus times for Oxfordshire at [http://www.oxontime.com](http://www.oxontime.com). I think the fact that it is possible to get these times is fantastic, but I think it's really disappointing that the user experience on that site is so poor. It should be easy to get times for nearby stops.

So I decided to make it easier, by building a better site.

### Getting the stop times ###
Unfortunately, there's no easy way to get the data on Oxfordshire bus times - there isn't an API. So I wrote a Python module to scrape the consumer facing site: [BusScraper on PyPI](http://pypi.python.org/pypi/BusScraper/), [BusScraper on GitHub](https://github.com/matthewowen/busscraper). In a rare moment of fortune, the same system is used on a number of sites, which means this module works for Bristol, South Yorkshire, Kent etc as well as Oxfordshire. It offers a few functions, but the most important one is that if you give it a stop ID, it will give you the buses departing from that stop in the next hour.

For obvious reasons, this isn't very efficient and it is pretty brittle (if the markup on the consumer website changes, this scraper will stop working), but it is the only route available at the moment.

### Getting nearby stops ###
To figure out the nearby stops (so we can pass their IDs to the scraper), we have to do two things: figure out where the user is, and then figure out which stops are near that location. Thankfully, both are pretty simple to do. 

To get a user's location, we can just use [geo.js](http://code.google.com/p/geo-location-javascript/). It is delightfully simple, and takes the pain out of cross browser / platform location. use the getCurrentPosition method and it will try to do so, run different functions if it can or cannot find location, and make the user's lat/long available.

Wonderfully, it is possible to get [NapTAN data for the entire UK](http://data.gov.uk/dataset/naptan); this is all the points of access to public transport in the country, in a big CSV file. I took a subset of this (stops in Oxfordshire), trimmed it to the fields I need, and put it in an SQLite database. SQLite isn't fashionable, but it is useful; as the application only ever reads from the database, the fact that the database is just a file makes it easy to version control. To figure out distance and proximity, I'm just doing basic pythagorean trigonometry - the distances are so short that it isn't worth considering curvature, and at this long/lat an unsophisticated conversion of lat/long units to metres is sufficient for the purposes required.

### Bringing it together ###
So, you've visited [http://www.oxbus.co.uk](http://www.oxbus.co.uk). It has found your location. It has forwarded you to a URL with your location in the path, and found the ten nearest stops. For each of those stops, the application will use BusScraper to find the next buses. It'll then store that data in a redis cache for 60 seconds (any longer and the data stops being useful). This caching to help make heavy usage a little more efficient, and also means that the app doesn't have to do the scraping all over again if the user clicks through to get more info on a particular stop. 

It'll then display the stops in a list. Javascript is used to show/hide the later buses for a given stop - getting them all at once doesn't add much page weight, and avoids potentially time consuming requests to the server (ping times on mobile can be truly dreadful). If you want to see stops that are further away, you can scroll down to the bottom of the page and tap to show more. This'll fire an AJAX request to add in the next ten stops.

### Mapping ###
I started simple, but I always wanted to build it out more. The big step forward was introducing maps. By tapping 'show on map', you can see the stop's location on a map, and see directions from where you are to where the stop is. This makes it a little easier if you're out and about in an area that you aren't familiar with.