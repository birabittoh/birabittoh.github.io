+++
title = "Modern web bloat"
date = 2021-04-09
template = "blog-page.html"
[taxonomies]
tags = [ "foss", "minimalism" ]
+++

This is it. My first blog post; I suppose this officially makes be a boomer.

## Inspiration
Some time ago I stumbled upon a [video](https://invidio.us/cvDyQUpaFf4), where the popular Linux influencer [Luke Smith](https://lukesmith.xyz) talked about the effort of looking up a Chicken Parmesan recipe in 2021 without having any adblock or privacy extensions enabled.

That's because most modern websites take a lot of time to load framework files, ads and trackers. While that's kinda functional, I think we should change our habits and start making simple websites again.

Yeah, this looks like a first world problem and it probably is, but it's not as subtle as you think. I'm actually convinced that the internet could actually benefit from this way of thinking, and that's what I'm going to talk about.

## The problem
In the early days of the internet, it was common for webpages to be written using only HTML, so we had very ugly but functional websites.

As technology went on, sites needed to get more modern-looking and interactive; that's why CSS and JavaScript were introduced into the mix, allowing for dynamic websites that could actually change based on user input.
As of nowadays, a lot more stuff went into the mix, to the point where the browser is now the most common program we use in our OS: you can, in fact, use it for doing things that 15+ years ago required external programs, like:

* playing music and video,
* reading PDF files,
* doing office work,
* checking e-mail,
* cloud storage,
* etc...

I guess people just find it more comfortable if they can do everything with a single program, and they're not to blame for that. This IS the easiest approach for unexperienced people: just have a program that does everything, instead of having to learn how to use a bunch of different software.

This plethora of uses is possible today because of the existence of various libraries and frameworks that simplify JavaScript and CSS and make them easier to develop complicated websites with.
This is good for basic web users who just want functional websites, and great for developers since they can easily code advanced functions inside the browser, which makes them work in every OS.

Sadly, this brings us to the problem: any modern website has become a burden for any browser to load, since our browser needs to download and parse through each library and often fill the page content as you scroll through.
In his video, Luke Smith found that a simple Chicken Parmesan recipe would take up to 5-10 megabytes, which doesn't sound like a lot, but it actually is.

It's easier to understand it if you think about it with video-games; any game on 16-bit consoles and earlier, including full-fledged 30+ hour adventures like Final Fantasy 6 and Chrono Trigger, weighs less than one single recipe page (as stated [here](https://blogs.umass.edu/Techbytes/2014/02/10/history-of-gaming-storage/#attachment_2827)).

## The solution
Well, I don't think this "problem" is getting solved soon, as new frameworks for web development are constantly being introduced. Sadly, it's a one-way train, but if you're a web-dev you could actually make a difference yourself!

I mean, this can not apply to all websites. Some of them just NEED to be as responsive and interactive as they are; most of them actually just became bloated at a certain time period (probably mid-2000s) when having a flashy website was cool and different from what everyone else had.
Nowadays you can make a difference by using plain HTML and CSS for your website: this ensures your pages will load instantly and be compatible even with the oldest of browsers!
If you like this philosophy, you can check out other projects that aim for a simpler and faster web, like these ones:

* [gemini://](//gemini.circumlunar.space/): a new, purposefully limited, internet protocol;
* [based.cooking](https://based.cooking/): a modern recipe website based on user collaboration via GitHub;
* [wiby.me](https://wiby.me/): a search engine that aims to only index classic style webpages.
