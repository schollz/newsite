+++
title = "Thoughts on optimizing web apps for the Raspberry Pi"
date = "2016-02-17"
slug = "/optimizing-code-for-raspberry-pis/"
+++

I like to write web apps and host them myself on the Pi. I'm no code ninja. I write in Python which is a great language, but not the fastest or memory efficient. To overcome liabilities I've found some tricks. I'm sure these are probably obvious to most people so I'd love thoughts about other ideas for optimization!

### The problem: minimize RAM usage while maximizing speed
A pi has < 1 GB of memory and its never going to be as fast as a modern computer. Its crucial to minimize the memory so that you can have multiple apps on one pi (or lots of forks of one app) *and* its crucial maximize speed so the pi is not *noticeably* slower than a modern computer.

### Minimize RAM: Use databases, index everything
For all my web apps, I typically have a big dataset that I need to load as a resource, and while its tempting to load into memory to get huge speedups, I usually have to resort to a database so that I can keep things on disk and still get fast access.

Sqlite3 is great for databases. A great trick I've learned is to index your searches and try to use [indexed tables for your searches](https://www.sqlite.org/lang_createindex.html). Tons of performance with little overhead.

Plain text files sometimes come in handy as well. A trick for accessing data from a text file is to precompute the position of the data in the file and store the position in a keystore. Keystores can be fast, and seeking in a text file is faster than parsing it each time.

### Maximize speed: use CLI utilities, or use a fast language (last resort)
I'm no coding ninja - I avoid writing in C and have not even attempted assembly. I like Python. And usually I don't care whether my 9 millisecond Python function would run in 10 microsecond using C++. But for web apps, I do care if things start taking >1 second - which starts to happen when you iterate over 1,000,000 of something. This forces me to try to be more clever.

For simpler things I find there is a lot of power in finding CLI utility alternatives (e.g. [xarg for multiprocessing](https://pjps.wordpress.com/2011/09/17/multiprocessing-with-xargs), grep for searching, agrep for fuzzy searching, wget for web). These can go a long way, even after accounting for the extra time to call the subprocess.

For harder cases, Go has become my best friend as I find its straight forward to port to Python code,  its super lightweight (~10-20 MB RAM per executable) and its fast. Usually at least a 10x speed up from Python. This can make the difference between a noticeable 1 second delay and an imperceptible 100 millisecond delay in web rendering.

## tldr; use databases - but index them; take advantage of common utilities or learn Go
