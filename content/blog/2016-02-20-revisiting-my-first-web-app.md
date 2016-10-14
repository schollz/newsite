+++
title = "Revisiting my first web app"
slug = "/revisiting-my-first-web-app/"
date = "2016-02-20"
+++


![Music Suggestions Ninja](/assets/images/musicninja-april.JPG)

> #### _"There is no perfection, only beautiful versions of brokenness" - Shannon L. Alder_

About one year ago I wrote my first web app, the [Music Suggestions Ninja](/2015/04/25/my-first-web-app-a-music-recommendation-and-playlist-generation-service). It wasn't fancy and it wasn't fast. But it worked and [people liked it](https://www.reddit.com/r/InternetIsBeautiful/comments/3455li/type_any_band_name_and_get_a_link_that_plays/). I wrote it in Python. It used very little dependencies, but only because I didn't know about any of the great Python packages that I know of now. To make it work, I ended up [writing a homegrown fuzzy string matching library](/faster-string-matching/) and I learned a lot about NGINX loaded balancing. Still, it [suffered massively in production](/a-visualization-of-my-hug-of-death/index.html).

*But it worked*. Its been running for about a year now on my schools fancy virtual machine - a 2-core Intel Xeon 2.70GHz loaded with Ubuntu, 2GB of memory and 20GB of disk space. Since I would eventually lose access to this fancy virtual machine, I decided to rewrite this program knowing what I know now - **so that it functions perfectly on a Raspberry Pi 2** with a third the computing power, half the disk space and half the memory.

## We can rebuild. We have the technology.

Where to start to transform my old web app into something leaner and faster? Well, when I took a look at the old site ([hosted here currently](http://musicninja.duckdns.org/) and [archived if no longer available](https://web.archive.org/web/20160131203756/http://www.musicsuggestions.ninja/)), it carries a bunch of junk no one cares about or uses. No one reads the blog, or the changelog, or the about page, or the terms of use, or the contact info. No one uses the API. So, out they go. Once I removed the junk, I realized there were only two components left - generating band names from searches and saving playlists. Simple.

I rewrote everything in my new favorite language, Go following [some generic principles](/optimizing-code-for-raspberry-pis/). It took about 3 hours. Luckily,  I had just ported my [homegrown fuzzy string matching library from Python to Go](/really-fast-fuzzy-string-matching/) which had a 10x speed improvement already. From there I followed the same methods I used to write [my Golang wiki](https://github.com/schollz/awwkoala) to add routes and process inputs. Everything else is basically the same, except the backend database is now BoltDB instead of MySQL.

## Benchmarking the new vs old

I benchmarked both versions of Music Suggestions Ninja, the old and new, by searching for a band called "Coldpla" (which is fuzzy matched to "Coldplay"). The disk space was measured using `ds -sh ./`, the memory usage was measured using `free -m` before and after stopping program, and the speed was measured using [`locustio`](http://locust.io/) by simulating 300 users hatching 10 users/second. Here are the results:


| Version | Disk space | Memory usage | Speed |
| ++++++++++++++++++| ++++++++++++++++++| ++++++++++++++++++| ++++++++++++++++++|
| [Old](http://musicninja.duckdns.org/) | 480 MB | 580 MB | 1 req/sec |
| [New](https://www.musicsuggestions.ninja/) | 140 MB | 5-20 MB | 20 reqs/sec |


<br>
Basically, my **new version is 20x faster and uses 70% less disk space and 97% less memory.** _Note: the improvement is not strictly due to Python vs Go._ A lot of the improvement comes from implementing everything in a much smarter way (which comes from experience and is not language specific). E.g. I am using more DB files instead of loading into memory, I am skipping `for` loops that were unnecessary and using more REGEX.

## tldr;

One year ago, it wasn't possible for me to slim down my programs to allow the Raspberry Pi to run *one* web app. One year later, [I am hosting 4 web apps](/about/) on my Raspberry Pi thanks to knowledge and experience that comes with time (and lots of playing).
