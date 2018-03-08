---
layout: post
title:  Cincinnati Reds travel schedule
date:   2015-03-15 16:40:16
description: Using ArcGIS Pro to animate the Reds' road trips in 2015
author: Danny Berghoff
---
Major League Baseball players face a grueling regular season schedule of 162 games packed into 6 months between April and September. Flying back and forth across the country is part of the sport, and a few bad road trips can make or break a season. With that in mind, my goal for this exercise was to visualize the win/loss performance throughout the 2015 season for my hometown team, Cincinnati Reds. I also wanted to try out some web-scraping using Beatiful Soup for Python.
<br><br>
Some assumptions before we get started:
<ul>
	<li>Assumption 1</li>
	<li>Assumption 2</li>
	<li>Assumption 3</li>
</ul>
<h3>Scraping mlb.com to get stadium coordinates</h3>
<br>
For the sake of simplicity, I decided to use the address of each team's stadium as the start/end point for all travel. <a href="http://www.mlb.com/team/">MLB.com</a> has team by team information laid out in a nice grid. The only thing missing is a handy download link...I decided to try out web parsing using the Python package Beautiful Soup. This package allows you to parse the HTML codee of a web page and pick out the sections you need. A well-formatted web page will often have clear tags and classes that make it easier to identify specific sections.  In this case, the information for each team is placed within unordered lists. The teams are sorted by league into two classes--"al team" for American League and "nl team" for National League. I can simply use the Beautiful Soup parser to locate all instances of each class and return the information of the corresponding list elements.
<br>
{% highlight python %}
from bs4 import BeautifulSoup as bs
import requests
import csv

page = requests.get("http://www.mlb.com/team")
soup = bs(page.content, 'html.parser')

#American League
allist = []
for ultag in soup.find_all('ul', {'class': 'al team'}):
	list1 = []	
	for litag in ultag.find_all('li'):
		list1.append(litag.text)
	allist.append(list1)

#National League
nllist = []
for ultag in soup.find_all('ul', {'class': 'nl team'}):
	list1 = []	
	for litag in ultag.find_all('li'):
		list1.append(litag.text)
	nllist.append(list1)

with open("address2.csv", "wb") as f:
	writer = csv.writer(f)
	writer.writerows(allist)
	writer.writerows(nllist)
{% endhighlight %}
<h3>Geocode addresses</h3>
DUMMY TEXT
<h3>Download schedule results</h3>
DUMMY TEXT
<h3>Calculate distance</h3>
DUMMY TEXT
<h3>Plot stadiums</h3>
DUMMY TEXT
<h3>Create lines between games</h3>
DUMMY TEXT
<h3>Set up time functionality on routes layer</h3>
DUMMY TEXT
<h3>Create animation</h3>
DUMMY TEXT
