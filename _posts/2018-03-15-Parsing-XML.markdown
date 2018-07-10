---
layout: post
title:  Parsing XML Files
date:   2018-03-13
description: Using Python to process XML files of SMS text messages.
author: Danny Berghoff
---
In my latest attempt to clear space on my phone I decided to permanently delete the tens of thousands of text messages that had piled up (this was long overdue). Using the SMS Backup & Restore app for Android made this process very easy; I was able to quickly backup all my texts and calls as an XML file to a folder on Google Drive. I then became intrigued by the possibility of exploring these XML files--some conversations contained over two years of text messages! The XML file contains some juicy metadata for potential analysis including date/time, sender, recipient, and text. However, due to the large size of the file, my laptop continually crashed when trying to open with Excel or even a text editor. This led me to exploring options with Python. I started by writing a few lines to simply parse the file for each instance of an SMS message and dumped the metadata into a list.I found the <a href="https://docs.python.org/2/library/xml.etree.elementtree.html#module-xml.etree.ElementTree">ElementTree</a> module to be an excellent tool for parsing XML files.
<br>
{% highlight python %}
import xml.etree.ElementTree as ET
tree = ET.parse('sms-2018-03-12 22-53-40.xml')
root = tree.getroot()
{% endhighlight %}
<br>
This is how a single SMS message is stored within the XML file. Personal phone numbers have been replaced with x's:
{% highlight XML %}
<sms protocol="0" address="+1xxxxxxxxxx" date="1508531315289" type="1" subject="null" body="Heading your way now!! Be there in a few. " toa="null" sc_toa="null" service_center="null" read="1" status="-1" locked="0" date_sent="1508531313000" readable_date="Oct 20, 2017 4:28:35 PM" contact_name="Mom" />
{% endhighlight XML %}
<br>
Most of the metadata is fairly straightforward (e.g. "body", "readable_date", "contact_name"). A little deduction allowed me to figure out that the "type" field indicates whether the message was sent or received by my phone. Type of 1 indicates I sent the message, type 2 indicates I received the message from "contact_name".
<br><br>
The script below uses an iterator to parse every iteration of the "sms" tag. On each pass, the metadata from each message is stored as temporary variables and added to a list. The list of variables from each message is added to a master list, "mylist."
{% highlight python %}
mylist = []
for sms in root.iter('sms'):
	textlist = []
	date = sms.get('readable_date')
	contact = sms.get('contact_name')	
	sender_type = sms.get('type')
	sender = ''
	recipient = ''
	if sender_type == "1":
		sender = contact
		recipient = 'Danny'
	else:
		sender = 'Danny'
		recipient = contact
	text = sms.get('body')
	textlist.append(date)
	textlist.append(sender)
	textlist.append(recipient)
	textlist.append(text)
	mylist.append(textlist)
{% endhighlight %}
<br>
Printing one element from this list shows the following:
{% highlight python %}
['Jul 8, 2015 7:25:59 PM', 'Danny', 'Dad', 'Where are your seats?']
{% endhighlight %}
Storing the texts in the list in this way makes the data a bit easier to manage and highlights the information I am most interested in. Perhaps not the most elegant way to process the data (I see you, Pandas), but lists make sense to me and are simple to manipulate with this amount of data.
<br>
My next thought was to use my master list of text conversations to analyze word frequency. I could count exactly how often I use "haha" (it's a lot) or determine which of my friends are most often subjected to my swinging moods during Bengals games. 
