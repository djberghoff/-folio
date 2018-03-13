---
layout: post
title:  Parsing XML Files
date:   2018-03-13
description: Using Python to process XML files of SMS text messages.
author: Danny Berghoff
---
In my latest attempt to clear space on my phone I decided to permanently delete the tens of thousands of thousands of text messages that had piled up (this was long overdue). Using the SMS Backup & Restore app for Android made this process very easy; I was able to quickly backup all my texts and calls as XML files to a folder on Google Drive. I then became intrigued by the possibility of exploring these XML files--some conversations contained over two years of text messages! The XML file contained all the necessary metadata for potential analysis--date/time, sender, recipient, and text. The only problem was figuring out how to read the file; at over 0.5 GB in size, my laptop continually crashed when trying to open with Excel or even a text editor. When all else fails, I usually think to Python. I started off by writing a few lines to simply parse the file for each instance of an SMS message--no MMS (picture, group chat, etc)--and dump the metadata into a list:
<br>
The <a href="https://docs.python.org/2/library/xml.etree.elementtree.html#module-xml.etree.ElementTree">ElementTree</a> module is an excellent way to parse XML files.
{% highlight python %}
import xml.etree.ElementTree as ET
tree = ET.parse('sms-2018-03-12 22-53-40.xml')
root = tree.getroot()
{% endhighlight %}
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
