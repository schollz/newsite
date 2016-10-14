+++
title = "How to prevent link rot"
date = "2015-10-01"
+++

# What is "link rot"?

**Link rot** is real, it is common, and it is pervasive. Link rot is essentially *the process by which hyperlinks cease to function, usually because the web page or server they point to has moved or has become permanently unavailable*. It has existed since the internet began.

# Web pages are ephemeral

[Examples of link rot are far and wide](https://web.archive.org/web/20151001235130/http://www.informationr.net/ir/9-2/paper174.html).  [One study](http://dx.doi.org/10.1017/S1472669614000255) found that 70% of links in Harvard Law Review and 50% of links within the United States Supreme Court opinions are no longer viable. [1] If we are to take the internet as a primary resource seriously, then we seriously need to think about undertaking a better way of long-term preservation of link contents.

# Link rot is pervasive

There seems to be no top-level domains (TLDs) that are free of link rot. In [one study that looked at 720,000 pages](http://research.microsoft.com/pubs/73808/p97-fetterly.pdf) on a daily basis, in only 11 weeks, 15% of .net domains had rotted links, 10% of .com, .org domains had rotted links, and 5% of .edu links had rotted. [Another study](http://dx.doi.org/10.1002/bmb.2003.494031010165) found that within 24 months, 50% of .com domains and 20% of .gov domains were no longer viable. [2]

# What is the half-life of a link?

There are a number of sources that have looked into the [half-life](https://en.wikipedia.org/wiki/Half-life) of a website. Random web pages seem to have a half-life of about 2 years [3], legal citations 1.4 years [4], communication articles 3.2 years [5]. Digital libraries have a much higher persistance, as their half-life is about 24.5 years [6].

## How stop link rot

Link rot can be combated in several ways.

1. Use a [digital object identifier](http://www.doi.org/) which provides persistent and actionable identification. The a website under a DOI changes, it can be changed in the DOI service so that the unique DOI identifier always links to the most up-to-date material.
2. Use a [permanent web framework](http://ipfs.io/). Our current web protocol has many nodes of failure, while newer hypermedia frameworks seek a more distributed protocol with builtin content identies that would allow data to permanently be available.
3. Only link to archiving sites like the [Internet Archive](https://archive.org/web/) or [perma.cc](https://perma.cc/). **My little program I've written helps to combat link rot by providing a simple and easy way to convert all your links to an archived link.**

# My attempt at stopping link rot.

In an effort to try to stop link rot, I've created a simple web app that convers all links to archivable links. The code is available at [http://github.com/schollz/prevent-link-rot](https://github.com/schollz/prevent-link-rot). Here is a snapshot:

## Snapshot

![Screenshot](http://i.imgur.com/LSM8HUU.jpg)

# References


1. Zittrain, Jonathan, Kendra Albert, and Lawrence Lessig. "Perma: Scoping and addressing the problem of link and reference rot in legal citations." Legal Information Management 14.02 (2014): 88-99.
2. Markwell, John, and David W. Brooks. "“Link rot” limits the usefulness of web‐based educational materials in biochemistry and molecular biology*." Biochemistry and Molecular Biology Education 31.1 (2003): 69-72.
3. Koehler, Wallace. "Web page change and persistence—A four‐year longitudinal study." Journal of the American Society for Information Science and Technology 53.2 (2002): 162-171.
4. Rumsey, Mary. "Runaway Train: Problems of Permancence, Accessibility, and Stability in the Use of Web Sources in Law Review Citations." Law Libr. J. 94 (2002): 27.
5. Dimitrova, Daniela V., and Michael Bugeja. "The half-life of internet references cited in communication journals." New Media & Society 9.5 (2007): 811-826.
6. Nelson, Michael L., and B. Danette Allen. "Object persistence and availability in digital libraries." D-Lib magazine 8.1 (2002): 1082-9873.
7. Perkel, Jeffrey M. "The trouble with reference rot." Nature 521.7550 (2015): 111-112.
