---
layout: post
title:  "Automating web scrapper - Wunderground"
date:   2020-05-24 00:00:00 -0400
categories: Projects
tags: Projects Webscrapping Python3 Plotly Automation
description: Also plotting some data with Plotly! 
---
[Github Repository]
[Sample Graph]

-----

So if you were here at some point and noticed that my other webscrapping tutorial is down, it's because technically ['torrent'] itself is not illegal 
but the content that you torrent can be illegal... So instead I will be showing how to webscrape some data from [Wunderground]! And possibily graph it
nicely using [Plotly]!

-----

# Part 1: Webscrapping Wunderground

So essentially, webscrapping is simply the process of extracting information from a website by looking at elements in the source code (HTML) or at least 
that's how I'm implementing this program... 

So the first step I would take is to look for the specific 'elements' that I'm looking for. In my program, I will specify the url that contains the location that I
want to pull data from so I don't have to deal with GET and POST requests and all the other bullshit that wouldn't really take long to figure out but too lazy to 
figure out:) In my case, the url would be: https://www.wunderground.com/weather/us/ny/manhasset/11030. 

When you go to this url, you should see something like these photos. 

-----

![first]({{ site.baseurl }}/assets/images/2020-05-24-Automate-web-scrapping-weather/wunderground_first.PNG)
![second]({{ site.baseurl }}/assets/images/2020-05-24-Automate-web-scrapping-weather/wunderground_second.PNG)

-----

Now in the first photo, the data that I want to extract from here is the nice big 60 in the middle of that circle and in the second photo, the data I want to 
extract are pressure, visibility, dew point, humidity, rainfall, and snow depth. So now that I have my eyes on the prize, it's time to inspect element and find 
some unique references to help find these data values. I will only do it for the first one (temperature), but I'm sure you can figure the other ones out. 

-----

![third]({{ site.baseurl }}/assets/images/2020-05-24-Automate-web-scrapping-weather/wunderground_inspect_element.PNG)

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

-----

# Part 2: Automating webscrapper script

Because there are libraries that the python script needs and I don't want to install them globally, I created a virtual environment that will be sourced each time the script runs. 
In the script.sh, I first source the virtual environment that I created and installed the dependecies for my python script. I then enter the folder of the project. The reason I 
do this is because I was getting an issue where the pickle file woudn't be able to save in the directory that I specificed in the python script. Now after turning the python 
script into an executable, I run the script and then deactivate the virtual environment because I don't need it anymore. This script runs every 15 minutes on one of my servers 
via crontab. When using this script on your own machine, make sure you update the absolute path for your machine specifically. I also included the format of the crontab line 
that I used to run the script every 15 minutes. 

```bash
crontab -e
# insert -> */15 * * * * /home/helen/projects/weather_scrapper/script.sh
```

-----

# Part 3: Graphing collected data

I'm not too sure what I'm going to do with the data that I've been collecting but for now I decided to learn how to use Plotly and graph the data! After opening the pickle, 
I iterated through each row, appending the date and time (which I string formatted first), temperature, dew point, humidity, and rainfall into separate lists because 
these lists are going to be used as the y-values with respective to the date time. Afterwards, we add each individual 'trace', or line graph in this case, by stating the 
x-axis as the date-time string that was formatted beforehand and each y-axis as a different trace. 

```python
fig.add_trace(
    go.Scatter(x=date_time, y=temperature, name='Temperature',
               line=dict(color='royalblue', width=2))
)
fig.add_trace(
    go.Scatter(x=date_time, y=dew_point, name='Dew Point',
               line=dict(color='firebrick', width=2))
)
# etc...
``` 

Now, I got a little bored and decided that I wanted to be able to view the different graphs by adding a drop down menu. After a bit of searching, I used the 'update' method 
to update the layout of the graph. The thing to keep in mind is when updating the 'visible' list in args. The number of boolean values should be equivalent to the number of 
traces that you added before, within that order as well. For example, if you add a trace that plots date-time vs. temperature and then add another trace that plots date-time 
vs. dew point, then by specifying the 'visible' option as [True, True] will plot both graphs while [True, False] will only plot the date-time vs. temperature graph. The graph 
of the data that I've collected so far can be viewed [here]. 

['torrent']: https://en.wikipedia.org/wiki/BitTorrent
[Wunderground]: https://www.wunderground.com/
[Plotly]: https://plotly.com/
[Colab Research from Google]: https://colab.research.google.com/notebooks/intro.ipynb
[here]: https://public.sonbyj01.xyz/projects/weather_scrapping/temp-plot.html
[Sample Graph]: https://public.sonbyj01.xyz/projects/weather_scrapping/temp-plot.html
[Github Repository]: https://github.com/sonbyj01/weather_scrapper