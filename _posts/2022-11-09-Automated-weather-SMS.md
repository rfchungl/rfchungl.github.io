---
layout: single
title: "Python: Automated Weather Notification Using SMS"
description: "Scrapping weather from Google using Python then utilizing a SMS service to send customized messages based on the weather."
output: html_document
tags: [python]
comments: false
author_profile: true
toc: true
toc_sticky: true
---
### Description
When I arrived to the United States, there were days when I wore a sweater and jacket, and the day was not that cold. I felt very dumb walking down to my classes while other students were wearing t-shirts, shorts, and sandals. 
I know that we can check the weather on our cellphones. However, there is too much information like humidity, water density, weather variation between hours, air quality, etc. I wanted something simpler that could just tell me the weather condition and the temperature. I do not check my emails first thing in the morning, so I did not want them to be sent to my email. Instead, I wanted something quick like an SMS. 

#### What does this program do?
 This Python program scrap the weather data from Google depending on the location you put and is gathered through a few lines of Python code using the BeautifulSoup library. Then, it is sent to your phone through a SMS service (A token is needed for this).
------------------------------------------------------
### Code
** Things needed to run the Python code:**
```
- import schedule
- import requests
- from bs4 import BeautifulSoup
- from twilio.rest import Client
- Token from Twilio (They offer 15 days trial)
```

**Things needed to get my sms going**
```
account_sid = "insert account from twilio"
auth_token  = "insert token"
# Authenticator to get to twilio
client = Client(account_sid, auth_token)
```

**Defining the block of code as weathercondition**
```
def weathercondition():
# Weather city
	city = "Fayetteville"
# Creating url and requests instance
	soup = BeautifulSoup(requests.get(f'https://www.google.com/search?q=weather+in+{city}').text, "html.parser")
	#print(soup.prettify()) // this print function is disabled but I put it in here in case you want to see if the line is working.
```
**Catching what I need from beautiful soup and defining my variables**
```
	temperature = soup.find('div', class_= 'BNeawe iBp4i AP7Wnd').text # BNeawe iBp4i AP7Wnd is obtained by inspecting and finding the line that contains the temperature
	region = soup.find('span', class_= 'BNeawe tAd8D AP7Wnd').text # BNeawe tAd8D AP7Wnd is obtained by inspecting and finding the line that contains the region (Fayetteville, AR)
	day_and_weather = soup.find('div', class_= 'BNeawe tAd8D AP7Wnd').text # BNeawe iBp4i AP7Wnd is obtained by inspecting and finding the line that contains the day and type of weather
# Day_and_weather will print the day, hour, and the type of weather...I only need the weather condition so I split and print index 1
	condition = day_and_weather.split('\n')[1]
```

**Here is the condition needed to send me the sms. As long as the condition type is not 0, it will always send me a sms with the weather condition**
```
# Alternatively, if I wanted it to send me a sms when on rainy days, I could do that too
# Unfortunately, if you want to change the number to sent, you will have to pay as this is a free trial
	if condition != 0:
		message = client.messages.create(to="+14795026472", from_="+16084475947", body=f"Good morning, Ruben!\nWeather condition for today is {condition} and temperature is {temperature} in {region}.")
		print(message.sid)
		print("Message Sent!")
```

**Every day at a specific time will send me a sms reminder of the weather and weathercondition() is called. In my case, I will set it up when I wake up at 7:00AM**
```
schedule.every().day.at("07:00").do(weathercondition)
# As long as the condition is true, it will run
while True:
	schedule.run_pending()
```

---------------------------------

### Output
![output](https://user-images.githubusercontent.com/115122030/196613363-0caf92c0-1be7-43bc-9597-c05c4fc980a7.PNG)
*The end result is once you have the program running, everyday at 7:00 AM in the morning will send you a SMS stating how is the weather condition and what you need to wear depending on how you coded it.*
<br>

-----------------------------------------------------------
### Conclusion  
As you can see, with a few line of codes plus the authetication code from twilio we can create a personalized SMS reminder of the weather.  

---------------------------------------------------------------
### Python
[`Python File Here`](https://github.com/rfchungl/Projects-Portfolio/blob/main/Weather_Scrapping/Weather_Scrapper.py)  
The Python code has comments (pseudo-code) on what does each line do for a better understanding of the program. 
