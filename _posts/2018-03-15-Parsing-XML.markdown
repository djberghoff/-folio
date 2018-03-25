---
layout: post
title:  Parsing XML Files
date:   2018-03-13
description: Using Python to process XML files of SMS text messages.
author: Danny Berghoff
---
In my latest attempt to clear space on my phone I decided to permanently delete the tens of thousands of text messages that had piled up (this was long overdue). Using the SMS Backup & Restore app for Android made this process very easy; I was able to quickly backup all my texts and calls as an XML file to a folder on Google Drive. I then became intrigued by the possibility of exploring these XML files as some conversations contained over two years of text messages! The XML file contains some juicy metadata for potential analysis--date/time, sender, recipient, and text. However, at over 0.5 GB in size, my laptop continually crashed when trying to open with Excel or even a text editor. This led me to exploring options with Python. I started by writing a few lines to simply parse the file for each instance of an SMS message--no MMS (picture, group chat, etc)--and dumped the metadata into a list.The <a href="https://docs.python.org/2/library/xml.etree.elementtree.html#module-xml.etree.ElementTree">ElementTree</a> module is an excellent way to parse XML files.
<br>
{% highlight python %}
import xml.etree.ElementTree as ET
tree = ET.parse('sms-2018-03-12 22-53-40.xml')
root = tree.getroot()
{% endhighlight %}
<br>
Here is a snippet of what the XML file looks like. This is how a single SMS message is stored:
{% highlight XML %}
<sms protocol="0" address="+15133136006" date="1508531315289" type="1" subject="null" body="Heading your way now!! Be there in a few. " toa="null" sc_toa="null" service_center="null" read="1" status="-1" locked="0" date_sent="1508531313000" readable_date="Oct 20, 2017 4:28:35 PM" contact_name="Mom" />
{% endhighlight XML %}
<br>
Most of the metadata is fairly straightforward (e.g. "body", "readable_date", "contact_name"). A little deduction allowed me to figure out that the "type" field indicates whether the message was sent or received by my phone. Type of 1 indicates I sent the message, type 2 indicates I received the message (from "contact_name").
<br>
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
