+++
title = "How to scrape my site"
date = "2016-01-30T00:00:00.000Z"
+++

## Get the list of URLs
There is an easy way to do this. Simply go to the `sitemap.xml` to collect a list of the URLs. Here's a script that will download the sitemap and then parse them and output the list of URLs to `URLS.txt`.

```python
import os
import xml.dom.minidom

SITEMAP_URL = 'http://rpiai.com/sitemap.xml'

def get_first_node_val(obj, tag):
    return obj.getElementsByTagName(tag)[0].childNodes[0].nodeValue

if __name__ == "__main__":
    os.system('wget ' + SITEMAP_URL + ' -O temp')
    xmlParser = xml.dom.minidom.parseString(open('temp','r').read())
    urlset = xmlParser.getElementsByTagName("urlset")[0]
    urls = urlset.getElementsByTagName("url")

    with open("URLS.txt","w") as f:
        for url in urls:
            loc = get_first_node_val(url, "loc")
            f.write(loc.strip() + "\n")
```

## Download the URLs
The best way to do this is `wget`. This nice thing about `wget` is that it will keep the connection open, so you probably won't get 403ed into oblivion. Here's what I use to download the list:

```bash
 wget --no-clobber --content-disposition --trust-server-names -i URLS.txt
```

You can run this command multiple times and the `--no-clobber` will avoid overwriting things. The `--content-disposition --trust-server-names` will generate names based on the address.

## Compress the files
Since I don't like wasting space on my drive, I like to compress the downloaded files. I found that LZMA compression is really fast and gives high ratios, and its builtin to Python3 which makes later use a snap. You can do a couple of things to compress them:

```
find . -not -name "*.lzma" -type f -exec lzma {} \;
find . -not -name "*.lzma" -type f | xargs -n 1 -P 8 -I '{}' lzma '{}'
```

The second one is nice, as it will use 8 processor cores. These are also useful commands for moving lists of files.

Also you can easily find leftovers if you get stopped in the middle using

```
ls temp | grep html | awk -F'@' '{print $2}' | xargs -n 1 -I '{}' sed -i '/{}/d' URLS.txt
```
