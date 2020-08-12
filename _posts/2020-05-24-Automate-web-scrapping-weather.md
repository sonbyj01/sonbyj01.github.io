---
layout: post
title: Automating web scrapper - Wunderground
date: 2020-05-24 00:00:00
tags: [projects, webscrapping, python3, plotly, automation]
last_modified_at: 
---
-----

[Github Repository] -- [Sample Graph]

-----

So as it turns out, we're not in China and there are actually copyright laws that I have to abide by! Who would've known?

All jokes aside (for the most part), if you saw my previous blog at one point, you might have noticed that I took it down. (If you haven't seen it, 
disregard everything I've said up to this part, besides the part about China and copyright laws. After a good friend of mine strongly suggested that 
I take it down and implement the webscrapper on something else, I decided to write one for scrapping weather data!

Now I may write about the underlying motivation for this specifically but we'll just have to see...

Oh, and this would also be a great opportunity for me to try out [Plotly]! Just because it looks pretty damn cool compared to good ol' matlibplot. 

-----

# Part 1: Webscrapping Wunderground

So for those who don't know what webscrapping is, it's essentially scrapping things from the web... 

woaHHHHH really?! I didn't know that! Alright no need for your sarcastic comments. 

Webscrapping is simply the process of extracting information from a website by looking at elements in the source code (HTML)

or at least that's how I'm implementing this program... 

Alright cool, so how do I get started? Well the first step you should take is pick a website that you want to extract some data from. In my case, that will be 
[Wunderground], and more specifically the weather for Manhasset (just chosen out of random, absolutely coincidental...). Make note of that URL, so in my case it
will be: [https://www.wunderground.com/weather/us/ny/manhasset/11030](https://www.wunderground.com/weather/us/ny/manhasset/11030). Now technically I can script 
my program so it can send a POST request for the location that I want from the homepage of [Wunderground] but I'm lazy so I'm just going to hard code the URL. 

Now when you go to this url, you should see something like these photos. 

-----

![first]({{ site.baseurl }}/assets/images/2020-05-24-Automate-web-scrapping-weather/wunderground_first.PNG)
![second]({{ site.baseurl }}/assets/images/2020-05-24-Automate-web-scrapping-weather/wunderground_second.PNG)

-----

Okay cool cool cool, so now after roaming through the webpage, I've locked my focus on specific values that I want to pull: the temperature in the middle of the 
circle on top and most of the data from the 'ADDITIONAL CONDITIONS' section. With that in mind, I'm going to 'Inspect Elements' and find those values in the HTML 
code. From here, I can find unique attributes within the code, which is basically the driving force of this script. 

For all intents and purposes (and out of sheer laziness), I will only follow through with finding and pulling the temperature. But hey! Think about it this way: 
I'm giving you an exercise for you to practice this on your own! And don't worry, I'll be posting my code as well just in case you're in a rush to figure something out. 

-----

![third]({{ site.baseurl }}/assets/images/2020-05-24-Automate-web-scrapping-weather/wunderground_inspect_element.PNG)

-----

Awesome, so as you can see in the image above, the 60 that represents the temperature is tagged with 'span' and uses the class 'wu-value wu-value-to'. 

```html
<span class="wu-value wu-value-to" _ngcontent-app-root-c122="">60</span>
```

Noice! That means when I run my webscrapper, I should be searching for the tag 'span' with class value of 'wu-value wu-value-to'. Alrighty then, let's jump into the code!

-----

So the first thing I have to do is make a request to the url that contains the data that I want. Afterwards, I'm going to use BeautifulSoup to make the requested url 
into a usable and readable form. 

```python
URL = "https://www.wunderground.com/weather/us/ny/manhasset/11030"
response = requests.get(URL)
soup = BeautifulSoup(response.text, 'html.parser')
```

Now I need to actually find the data that I'm looking for in order to pull the correct data value. As I said before, the elements I probably should be looking for are 
'span' and 'wu-value wu-value-to'. Therefore, I will use the 'find' function to specify which elements/tags/stuff I am looking for. I will then print out what is has 
found so I can see if the data I want is part of the results. After printing the 'temperature_results', I quickly noticed that the first element of the array contains 
the temperature that I want, so I store the zeroth index into 'temperature'. (If you thought it would be 1, this ain't for you because this ain't matlab bud; 
matlab is the wack one for indexing arrays at 1). 

```python
temperature_results = soup.find('span', class_="wu-value wu-value-to")
temperature = str(temperature_results.contents[0])
```

Anddd boom! Now I have successfully taken the temperature data from Wunderground! Just repeat the same process with the other data that you want to extract. However, I 
will make a note that for the other ones, you may need to do '.contents' twice... Just sayin' man... 

And another note, I would highly recommend writing this program using Juypter notebook first because it will enable you to run sections of your code quickly, instead of 
making new request each time you run the entire program again. If you don't have access to Juypter notebook from school or something, I would suggest using [Colab Research 
from Google]. (Not sponsored by Google, not important enough...) I originally wrote this script using Colab Research but then moved all my code into regular python 
because I wanted to run this as a executable script (as you can see with my SHEBANG at the top). 

Now there are a bunch of other lines of code that I included in my scrapper.py program and those are mostly for me and my purposes, but I'm sure you can figure out what I'm 
doing anyways:)

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

And there you have it! That's Webscrapping 101! 

-----

# Part 2: Automating webscrapper script

Now I have a small little desktop that's acting as my 'server' (I only call it that because I never shut it down). It currently runs CentOS... 7? 8? 9? 10? Eh, at least I 
remember how to count. Anywho, my plan was to run this script every 15 minutes so I can collect data at that interval. So to go about this, I decided to just use the default 
cron utility. However, one of the good practices that I've learned over the years is to create virtual environments for my python projects, instead of installing them directly 
to the global directory. The issue is that I don't know how to activate the virtual environment and then run my script and then turn it back off... Unless I create a bash script!

So that's exactly what I did. Essentially in this bash script, I am turning on the virtual environment that contains the installed packages needed for the program, I run the program, 
and then I turn off the virtual environment. And this all happens every 15 minutes! Now I will let the record show that I was running into an issue where if I ran the script with its 
absolute path, it won't actually allow the program to save the pickle... Wack man... So I instead decided to just change directories into the project folder and run the script 
relatively.

```bash
crontab -e
# insert -> */15 * * * * /home/helen/projects/weather_scrapper/script.sh
```

And now boom, you have a thing collecting weather data every 15 minutes!

-----

# Part 3: Graphing collected data

By this point, I've sort of lost my purpose... I, more or less, just wanted to graph some stuff and play around with Plotly. So, I wrote a separate program called 'graph.py' 
that does exactly that! In the program, after opening the pickle, I iterated through each row, appending the date and time (which I string formatted first), temperature, 
dew point, humidity, and rainfall into separate lists because these lists are going to be used as the y-values with respective to the date time. Afterwards, I added each 
individual 'trace', or line graph in this case, by stating the x-axis as the date-time string that was formatted beforehand and each y-axis as a different trace. 

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

And there it goes ladies and gentlemen, that's it! 

If you liked this post, make sure you give it a thumbs up and smash that subscribe butto... Oh wait... Wrong platform... 


Thanks for reading - 

sonbyj01

['torrent']: https://en.wikipedia.org/wiki/BitTorrent
[Wunderground]: https://www.wunderground.com/
[Plotly]: https://plotly.com/
[Colab Research from Google]: https://colab.research.google.com/notebooks/intro.ipynb
[here]: https://public.sonbyj01.xyz/projects/weather_scrapping/temp-plot.html
[Sample Graph]: https://public.sonbyj01.xyz/projects/weather_scrapping/temp-plot.html
[Github Repository]: https://github.com/sonbyj01/weather_scrapper