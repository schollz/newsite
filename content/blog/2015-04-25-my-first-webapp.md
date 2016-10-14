+++
title = "My first web app - a music recommendation and playlist service"
slug = "/2015/04/25/my-first-web-app-a-music-recommendation-and-playlist-generation-service"
date = "2015-04-25"
+++

<p><a href="http://www.musicsuggestions.ninja"><img src="https://rpiai.files.wordpress.com/2015/04/music-ninja.jpg?w=640&#038;h=295" alt="music ninja"   class="aligncenter size-large wp-image-351" /></a></p>
<h1>The Story of <a href="http://www.musicsuggestions.ninja">Music Suggestions Ninja</a></h1>
<p>I have long been inspired by tools that can find new music, like <a href="http://www.gnoosic.com/">Gnod&#8217;s self-adapting system of learning</a>, <a href="http://www.last.fm">Last.fm music recommendations</a>, and the <a href="http://hypem.com/popular">Hype Machine trending aggregator</a>. I decided to try to make my own &#8211; a large task considering I know almost nothing about web applications. The result is the <a href="http://www.musicsuggestions.ninja">Music Suggestions Ninja</a>, so named only because the domain was cheap.</p>
<h2>The concept</h2>
<p>My concept was simple. I only wanted to have a button to press to generate a playlist of music that is very closely related to a given playlist. The task would consist of several steps though, that are actually slightly complicated.</p>
<h2>The problems</h2>
<p>Here are the problems that I ended up needing to solve:</p>
<p><strong><br />
1) I would need a list of band names and genres.<br />
2) The bands list needs to be clustered according to genre.<br />
3) I would need a way to search band names that can even allow for mispelling (gotta set high standards!).<br />
4) I would need a way to find music from the related bands in youtube<br />
5) I would need to create a web app<br />
6) Ideally I can host this on a Raspberry Pi.<br />
</strong></p>
<p>Most of these things are slightly technical and boring and tedious, but I&#8217;ll go over some things that I thought were kind of cool and interesting and possibly (though only slight) useful to the rest of you. If there are some things I don&#8217;t go over, that you would like to know more about, please comment and let me know!</p>
<h2>The solutions (selected)</h2>
<h3>A way to search band names that can even allow for mispelling</h3>
<p>So given I have a list of band names, how on earth do I search for one? I want to allow for mispellings because I want to always at least <em>try</em> to find something. This turns out, to be pretty easy. There is a pretty reliable and fast algorithm for computing the <a href="https://en.wikipedia.org/wiki/Levenshtein_distance">Levenshtein distance</a> which can essentially tell you the edit distance between two words.</p>
<p>There are problems with just using edit distance, though, as the edit distance can be quite misleading if the words are only just rearranged. However, someone smart has already thought of this and fixed it! The solution is to use the Python package <code>fuzzywuzzy</code> which can compute Levenshtein distances and account for all sorts of stuff, like rearrangements.</p>
<p>So I like to use two metrics, as I found they work better than one, so my string matching code is simply:</p>
<pre class="brush: python; title = ; notranslate" title="">
from fuzzywuzzy import fuzz
from fuzzywuzzy import process
import Levenshtein

def compareStrings(strings):
    leven1 = fuzz.token_set_ratio(strings[0],strings[1])
    leven2 = Levenshtein.ratio(str(strings[0]),str(strings[1]))
    return (strings[0],strings[1],leven1,leven2)
</pre>
<p>Now how to get the best match? Here&#8217;s another problem: their can be a lot of band names so its very slow to use a single loop. Here&#8217;s the solution: use more computer power! We can actually use the multiprocessing library to efficiently go through the band names, such as:</p>
<pre class="brush: python; title = ; notranslate" title="">
import operator
from multiprocessing import Pool

def printTopTenResults(searchString):

    stringList = []
    for bandName in bandNames:
        stringList.append((searchString,bandName))

    pool = Pool(5)

    results = pool.map(compareStrings, stringList)
    print (sorted(results, key=operator.itemgetter(2, 3), reverse=True))[:10]
</pre>
<p>Try it out! Combine the two and test it with a tiny list of band names:</p>
<pre class="brush: python; title = ; notranslate" title="">
bandNames = [&#039;Lynyrd Skynyrd&#039;,&#039;The Sex Pistols&#039;,&#039;Van Halen&#039;,&#039;Metric&#039;,&#039;Prince&#039;,&#039;Kings of Leon&#039;,&#039;The Beatles&#039;,&#039;The Monkees&#039;,&#039;Van Morrison&#039;,&#039;Jim Morrison&#039;]
printTopTenResults(&#039;Van Morrison&#039;)
</pre>
<p>So if you search for &#8220;Van Morrison&#8221; you&#8217;ll get a top result of <code>('Van Morrison', 100, 1.0)</code> followed by <code>('Jim Morrison', 80, 0.75)</code>, and <code>(Van Halen', 50, 0.47619047619047616)</code> as the respective scores (second and third in tuple) decreases. It works pretty well! I&#8217;m happy with it at least &#8211; its fast, efficient, precise, and scalable.</p>
<h3>A web app that allows more than one person to connect</h3>
<p>As I started looking at web apps I immedietly settled for a simple but elegant solution &#8211; using <a href="http://www.electricmonk.nl/log/2007/11/03/python-web-app-server-in-45-lines/">Electric Monk&#8217;s tinywapp</a> which is a really elegant a nice piece of code. It worked too, I could easily use it for what I&#8217;m doing, but I ran into a problem. It takes about 3-10 seconds to find the band name, generate the tree, and find the YouTube URLs. If another person connected, they would have to wait. So it was limited to about 6 people per minute.</p>
<p>The way around this problem is to a WSGI HTTP Server that can handle multiple workers. I used <a href="http://gunicorn.org/">gunicorn</a> but I&#8217;m told that their are others out there. This way I can spawn multiple processes, so that several people can connect at once! I also adapted this for my <a href="http://www.poetrygenerator.ninja/">poetry generator</a> which I found had a similar problem. The code for the server and the deploying process can be found <a href="http://github.com/schollz/poetry-generator">at my GitHub</a>.</p>
<h4>I realize I haven&#8217;t gone over all the solutions to the problems up top, but I can do that in later posts if people are interested!</h4>
