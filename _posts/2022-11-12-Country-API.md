---
layout: single
title: "Python: Getting University Names Based On The Country Inserted"
description: "Combining Python and API to generate university names once the name of the country is inserted"
output: html_document
tags: [python]
comments: false
author_profile: true
toc: true
toc_sticky: true
---
### Description
Using API and JSON library to create a Python program.  

### What does this program do? 
The Python code uses an API that shows you the college names based on the country you input. Once the user type the country name, it asks the API to retrieve the college names, in order to save them, we gather the results in JSON then use python dictionary to print the results.  

-----------------------------------------------
### Code
**The Python code has comments on what does each line do for a better understanding of the program.**  
<br>
**Things needed to run the Python code:**
```
import urllib.request
import json 
```

**Insert country**
```
print('BROWSE UNIVERSITIES FROM DIFFERENT COUNTRIES')
# as long as the user doesn't type x then this program will keep running
while True:
    # ask user to enter a country
    country = input('Enter country or enter "x" to exit: ')
    # if user inputted letter x then end the program
    if country == 'x': 
        quit() 
```

**Url of the API + the input of what the user typed. In this case, the name of the country**
```
    url = "http://universities.hipolabs.com/search?country=" + country

    # url can't have space, I added this so that the user can input countries with space in between such as United States, United Kingdom, Saudi Arabia, United Arab Emirates, etc
    urlreplace = url.replace(" ","%20")
```

**Requesting JSON data from the API**
```
    json_data = urllib.request.urlopen(urlreplace)
```

**Convert the JSON to Python Dictionary**
```
    data = json.loads(json_data.read()) 
```

**If user doesn't type the country name correctly, it will keep prompting "Please enter a valid country"**
```
    if len(data) == 0:
        print('Please enter a valid country!')
    else:
        for item in data:
            print(item['name'])
```

------------------------------------
### Output
![image](https://user-images.githubusercontent.com/115122030/197109556-754f44ee-aded-4dd7-84c3-ba5494afdacc.png)  

--------------------------------------
### Python
[Python File Here](https://github.com/rfchungl/Projects-Portfolio/blob/main/API/API.py)  

