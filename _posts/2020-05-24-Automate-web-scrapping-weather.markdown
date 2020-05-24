---
layout: post
title:  "Automate web scrapping - Wunderground"
date:   2020-04-19 00:00:00 -0400
categories: Projects
tags: Projects Webscrapping Python3 Plotly Automation
description: Also plotting some data with Plotly! 
---
So if you were here at some point and noticed that my other webscrapping tutorial is down, it's because technically ['torrent'] itself is not illegal 
but the content that you torrent can be illegal... So instead I will be showing how to webscrape some data from [Wunderground]! And possibily graph it
nicely using [Plotly]!

# Part 1: Webscrapping Wunderground

So essentially, webscrapping is simply the process of extracting information from a website by looking at elements in the source code (HTML) or at least 
that's how I'm implementing this program... 

So the first step I would take is to look for the specific 'elements' that I'm looking for. In my program, I will specify the url that contains the location that I
want to pull data from so I don't have to deal with GET and POST requests and all the other bullshit that wouldn't really take long to figure out but too lazy to 
figure out:) In my case, the url would be https://www.wunderground.com/weather/us/ny/manhasset/11030. 

When you go to this url, you should see something like these photos. 

-----

![first]({{ site.baseurl }}/assets/images/2020-05-24-Automate-web-scrapping-weather/wunderground_first.PNG)
![second]({{ site.baseurl }}/assets/images/2020-05-24-Automate-web-scrapping-weather/wunderground_second.PNG)

-----

Now in the first photo, the data that I want to extract from here is the nice big 60 in the middle of that circle and in the second photo, the data I want to 
extract are pressure, visibility, dew point, humidity, rainfall, and snow depth. So now that I have my eyes on the prize, it's time to inspect element and find 
some unique references to help find these data values. I will only do it for the first one (temperature), but I'm sure you can figure the other ones out. 

-----

![third]({{ site.baseurl }}/assets/images/2020-05-24-Automate-web-scrapping-weather/wunderground_second.PNG)

-----

As you can see in the image, the 60 that represents the temperature is contained within the element 'span'. 

```html
<span class="wu-value wu-value-to" _ngcontent-app-root-c122="">60</span>
```

Therefore, when I run my webscrapper, I should be searching for the element 'span' with class value of 'wu-value wu-value-to'. Alrighty then, let's jump into the code!

So the first thing I'm going to do is make a request to the url that contains the data that I want. Afterwards, I'm going to use BeautifulSoup to make the requested url 
into a usable and readable form. 

```python
URL = "https://www.wunderground.com/weather/us/ny/manhasset/11030"
response = requests.get(URL)
soup = BeautifulSoup(response.text, 'html.parser')
```

Now I need to actually find the data that I'm looking for in order to pull the correct data value. As I said before, the elements I probably should be looking for are 
'span' and 'wu-value wu-value-to'. So, I will find all elements that satisfy these two conditions. After printing out these found elements, I figured that I only need 
the first element of that array, so that's exactly the data I will take away from this. 

```python
temperature_results = soup.find('span', class_="wu-value wu-value-to")
temperature = str(temperature_results.contents[0])
```

And now I have successfully taken the temperature from Wunderground! You can figure out the rest, it's essentially the same process, though you may need to use .contents
twice instead of once... Also, I highly recommend doing this on some version of Juypter notebook because it was extremely easy for me to run a line or two of code instead of
rerunning the program over and over again. If you don't have Juypter notebook access from school or whatnot, then you can use [Colab Research from Google]. I originally wrote
this program using Colab Research but then moved all my code into regular python because I wanted to run this as a script. 

Now if you take a look at the scrapper.py program that I wrote, you'll notice a whole bunch of other lines that I wrote that I didn't explain yet. Well just deal with it and 
figure it out on your own, I'm sure you got this;)

```python
try:
    old_data = pd.read_pickle('./weather_data.pickle')
except FileNotFoundError as fnf:
    old_data = pd.DataFrame()
```

I wanted to have one pickle file that stores all the data that the scrapper collects each time it runs. I used specifically pickle because (1) there was already a built-in 
function to read and dump to pickle from pandas and (2) it would preserve all the metadata that I dump and read. Now the reason why I used pandas is because I wanted to be 
one of those kool kids in data science that use pandas #swag. In here, I check if there's already an existing pickle file. If there is, then I want to load those old data 
values because I will eventually append the new data onto the old data. If not, then just initialize a new data frame. 

```python
data = {}

# gets current date and time
now = datetime.now()
data['Day'] = [now.strftime('%d')]
data['Month'] = [now.strftime('%m')]
data['Year'] = [now.strftime('%Y')]
data['Hour'] = [now.strftime('%H')]
data['Minute'] = [now.strftime('%M')]
```

Because in the end I want to graph the data that I collected with respect to time, I stored the current time and date into a dictionary of data that would eventually 
be appended to a data frame (ooooHHH pandas, am I kool kid yet?). 

```python
df = pd.DataFrame(data)
# sees if there's previous data and append, otherwise move on
if old_data.empty:
    old_data = df
    # print('empty')
else:
    old_data = old_data.append(df, ignore_index=True)
    # print('not empty')

# stores into pickle file
# print(old_data)
old_data.to_pickle('./weather_data.pickle')
```

You know thinking about it, I thought I wrote nice and short commments that described what I was doing but screw it, I already made it this far. So now that I have 
a dictionary that contains all the weather data, date, and time, I finally convert it to a data frame. I then see if there's already data within the previous data frame; 
if so, then append, if not, then reassign. And finally, send the entire data back to the pickle file. 

# Part 2: Automating webscrapper script

# Part 3: Graphing collected data











['torrent']: (https://en.wikipedia.org/wiki/BitTorrent)
[Wunderground]: (https://www.wunderground.com/)
[Plotly]: (https://plotly.com/)
[Colab Research from Google]: (https://colab.research.google.com/notebooks/intro.ipynb)