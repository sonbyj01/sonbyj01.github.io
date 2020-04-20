---
layout: post
title:  "Web Scrapping BabyTorrent to Transmission (1/2)"
date:   2020-04-19 00:00:00 -0400
categories: Projects
tags: Projects, Webscrapping, Python3
description: Web Scrapping BabyTorrent to Transmission
---
So with a Plex Media Server set up and all, the whole point is to stream movies and tv shows from it, right? 

Wrong. 

The whole point is to show you how painful it is to make a docker container work and how irritating it is set up nginx with ssl certification 
so you don't look like a degen for having that red slash across that little lock you see next to the url and how annoying it is to download 
all those movies and tv shows you all so cherish. 

Anyhow, enough of all that. 

There's this great website called [babytorrent.se] where it contains a plethora of movies and series for you to download and enjoy!

Only problem (or inconveince, I don't know you pick) is that for certain shows like The Outsider or NCIS, there are so many links for the 
different episodes in a season. I don't know about the rest of you, but I would rather automated the tedious work. So, in the spirit of 
small projects and bettering my coding skills, I thought it would be a great idea to create a webscrapper!



![SoManyLinks]({{ site.baseurl }}/assets/images/2020-04-19-Web-Scrapper-1/Torrents.PNG)



So let's get started. I only had a general idea of how web scraping worked but decided to look up an article or two on how to actually implement one 
in python. Thankfully I came across this [little guide] that was more than enough to kick-start this whole shit show. 

-----

So after reading through the article and starting to code, I already ran into an issue. Great... Just my day... 

So if you click on the website, you'll notice that first it shows you a page that says " Checking your browser before accessing babytorrent.se ". Oh 
joy. There's a DDoS protection shit that the website uses from Cloudflare. Fine, I guess good for them. I knew I needed to bypass this somehow and I 
was almost sure someone thought of something. DuckDuckGo away! 



![CloudflareDDoSBullshit]({{ site.baseurl }}/assets/images/2020-04-19-Web-Scrapper-1/Cloudflare.PNG)



[Boom.] I was right. 

So after taking a look at the README.md, I made a small modification to the article's code and proceeded. All was good... 

```python
session = requests.Session()
scraper = cfscrape.create_scraper(sess=session, delay=10)
response = scraper.get(link).content
```

So after writing the "get_forwarding_links" and "get_magnet_links" methods (or so I thought they worked...), I ran into another problem!

-----

Back track a bit, not only did I want a webscrapper to collect all the magnet links into a list, I also wanted my program to shove all that into 
my transmission container that I have running on one of my servers. Because I wanted to access the transmission GUI from anywhere though, I did some 
nginx reverse proxy magic and boom, [I got it online]! I even put a username and password to it so no one can just log in and torrent a bunch of shit. 

Okay so back to the problem. Oh yeah. Username and password. Authentication.

But no worries! DuckDuckGo here to save the day!

[Shakalakabang.] Some useful information on authentication! Now after reading through that short page, I quickly opened up the built in network analyzer 
thing built into Firefox. After typing the url of my transmission site and clicking cancel when it asked for my credentials, I saw the first row in the 
analyzer. Right there, it said " www-authenticate: Basic realm="Transmission" "



![Network Analyzer]({{ site.baseurl }}/assets/images/2020-04-19-Web-Scrapper-1/Network.PNG)



So now I know transmission uses basic authentication. On top of that, the article also provides the code to use a GET request with the url and your 
credentials! Ah, life is so wonderful and sweet. As soon as I logged into my transmission site, I was going to go right into 'Inspect Elements'. However, 
I felt like there was an easier way of uploading multiple links at the same time. I thought I would try using the same technique of network analyzing as I 
did before, so I went ahead and tried. After looking through a couple of requests, I didn't see anything that struck me useful. So, being the clueless and 
extra person I am, I pulled out Burp Suite. 

-----

After configuring my computer to send all traffic through Burp Suite via proxy, I viewed the packets that were being sent around. Interesting enough, I did 
find a POST request packet that had a method of "torrent-add". Mhmm interesting... It also showed the magnet url that I previously requested for. So I knew 
this was it! After adding a couple more lines of code to make the POST requests, I was done! 



![CoolBurpSuite]({{ site.baseurl }}/assets/images/2020-04-19-Web-Scrapper-1/Burp_Suite.PNG)



```python
# Payload that contains all the information needed to complete a POST request for a magnet link
payload = {
    'method': 'torrent-add',
    'arguments': {
        'paused':'false',
        'download-dir':'/downloads/complete-tv',
        'filename': link
    }
}
```

FINALLY! I'M DONE! AFTER 3 HOURS OF DUCKDUCKGOING AND CODING AND JUMPING ONTO MY BED OUT OF FRUSTURATION, I WAS DONE!

Sitting happily in my chair, I ran my program after putting all the pieces together. 

My program finished running. 

I quickly checked my transmission website and there it was, 

A.B.S.O.L.U.T.E.L.Y. N.O.T.H.I.N.G.

Well shit. 

I'm putting a pin on this one. Till next time!

Thanks for reading -  
sonbyj01

*If you're curious to see what I have so far:*
```python
#!/usr/bin/env python3

import requests
from requests.auth import HTTPBasicAuth
import time
from bs4 import BeautifulSoup
import cfscrape

TRANSMISSION_URL = "https://transmission.sonbyj01.xyz/transmission/rpc"
TRANSMISSION_USERNAME = "" # almost got me there buddy
TRANSMISSION_PASSWORD = ""

def scrape_em(links):
    # links from the text file
    for link in links:
        # retrieves magnet links from one babytorrent site
        upload_to_transmission(get_magnet_links(get_forwarding_links(link)))

        time.sleep(1)

def upload_to_transmission(links):
    response = requests.get(
        TRANSMISSION_URL,
        auth=HTTPBasicAuth(
            TRANSMISSION_USERNAME,
            TRANSMISSION_PASSWORD
        )
    )
    for link in links:
        payload = {
            'method': 'torrent-add',
            'arguments': {
                'paused':'false',
                'download-dir':'/downloads/complete-tv',
                'filename': link
            }
        }
        requests.post(
            TRANSMISSION_URL,
            headers=response.headers,
            json=payload
        )

def get_forwarding_links(link):    
    session = requests.Session()
    scraper = cfscrape.create_scraper(sess=session, delay=10)
    
    if scraper.get(link).ok:
        response = scraper.get(link).content
        soup = BeautifulSoup(response, 'html.parser')
        a_class = soup.findAll('a')
        urls = []

        for a in a_class:
            try:
                url = a['href']
                if url.find('https://sportsat.online/') != -1:
                    urls.append(a['href'])
            except:
                pass
    return urls

def get_magnet_links(links):
    for link in links:
        session = requests.Session()
        scraper = cfscrape.create_scraper(sess=session, delay=10)
        time.sleep(10)
    
        if scraper.get(link).ok:
            response = scraper.get(link).content
            soup = BeautifulSoup(response, 'html.parser')
            a_class = soup.findAll('a')
            urls = []

            for a in a_class:
                try:
                    url = a['href']
                    if url.find('magnet') != -1:
                        urls.append(a['href'])
                except:
                    pass
    print(urls)
    return urls
            
def main():
    # file_name = input("Enter text file containing 'babytorrent.se' urls: ")
    file_name = 'urls.txt'

    try: 
        text_file = open(file_name, 'r')
        links = text_file.read().splitlines()
    except FileNotFoundError as fnf_error:
        print(fnf_error)
        exit(0)

    scrape_em(links)
    
if __name__ == "__main__":
    main()
```


[babytorrent.se]: https://babytorrent.se
[little guide]: https://towardsdatascience.com/how-to-web-scrape-with-python-in-4-minutes-bc49186a8460
[Boom.]: https://github.com/Anorov/cloudflare-scrape
[I got it online]: https://transmission.sonbyj01.xyz/
[Shakalakabang.]: https://requests.readthedocs.io/en/master/user/authentication/ 