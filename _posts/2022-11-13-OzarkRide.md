---
layout: single
title: "Business Case: OzarkRide"
description: "Using Excel and SAS to perform business and statistics analysis such as ANOVA and regressions to help OzarkRide to expand."
output: html_document
tags: [excel, sas, analysis]
comments: false
author_profile: true
toc: true
toc_sticky: true
---
### Business Scenario  
OzarkRide is a local bike rental founded in 2011 from Fayetteville which focuses on a 
sustainable and affordable way to reach from point A to point B using environment-friendly 
technology. OzarkRide is planning to open a new branch in a city similar to Fayetteville within a 
few months. They have hired us at Fay Consulting to dive deep into their data and provide 
insights on the findings.  
<br>
One of the main and most important questions they have is which month should they
launch their service? Is there anything other than the month they should consider? Support 
with analytics. 

------------------------------------------------------------
#### Response 
<br>
There are more than months that OzarkRide should consider when planning to open a 
new branch in a different city. At Fay Consulting, we believe that besides months, seasonality, 
weather, and population should be considered. As seasons are tied with the weather, by 
common sense we know that when it is cold outside, people do not do many outdoor activities 
and rather have indoor activities. The same when it rains; people try to stay dry and will not be 
riding a bike.  
<br>
During our research, we found that Fall and Summer are the best time to target
expansion. Based on the operation data, Fall has the highest number of users followed by 
Summer. We believe that beginning the summer the month of - May through October when the 
fall season ends - is the peak time to expand the operation in a new city.  
<br>
Knowing the population density also plays a very important role in the success of the 
expansion. Fayetteville is a college town. Therefore, there is a high density in population during 
school time resulting in more people using the bike rental service. Depending on where 
OzarkRide plans to expand, a study of the population should be performed before making a 
decision. Unfortunately, the dataset provided does not have the information necessary to 
analyze the population.  

#### Recommendation  
Our recommendation at Fay Consulting for OzarkRide is the following:  
* Have in mind to expand either during Summertime or during Fall. The earlier OzarkRide 
expand, the more exposure to the population.  
* The best months to expand is in the range between May and October; we believe that a 
month in the early middle of the range is the best-case scenario. In this case, the 
months would be May, June, July, and August based on the analysis performed. The 
later OzarkRide goes, the less exposure the company has to new customers due to 
competition.  
* Perform further analysis to study the population of the city where OzarkRide wants to 
open the new branch. This is key if OzarkRide wants to have higher traffic in the bike 
renting service.  
* Study possibly competitions that provide similar services as OzarkRide. The more service 
varieties the targeted city has, the fewer customers the company gets in the launch
unless a strong marketing campaign with promotions is included.  

-------------------------------------------------------------------------------------

### Appendix   
#### Summary Statistics
![Summary Statistics](\assets\images\summary statistics.PNG){: .align-center}
* The season with the highest average mean is Fall followed by Summer.
* The season with the lowest average mean is Spring. _(This is skewed because the data captured is wrong…January and February are considered winter, but the data has it as 
spring)_  
<br>
<br>
![Total Users by Months](\assets\images\total users by months.JPG){: .align-center}  
* May through October have very similar number in total users with August being the highest by just 5,000.  
<br>
<br>
![Representation of Users by Seasons](\assets\images\representation of users by season.JPG){: .align-center}    
* We clearly see that Fall has the highest representation of users followed by Summer.  
<br>
<br>
![Users by Temperature](\assets\images\user by temperature.JPG){: .align-center}   
* Low temperature means less people using OzarkRide service. On the other hand, as the temperature increase, the amount of rental increase for both casual and registered users.  

#### Visualization and Descriptive Take Away  
* Having the highest number of users during Fall and Summer is very normal. The weather during those two seasons is warm and people usually do outdoor activities in this case, biking.  
* The reason why January and February have the lowest number of users is that it is the beginning of cold weather and people prefer to stay home or do indoor activities. Unfortunately, the data capture has spring starting in January and February, thus, skewing the spring data in favor of winter.  
* OzarkRide has two types of clients: casual users and registered users. Casual users make up about 18.8% of the revenue while registered users make up about 82% of the company's revenue. Although the difference between casual users and registered users is alarming having 82% registered users is a very good number.  

------------------------------------------------------
### Hypothesis Testing 
#### ANOVA Testings 
_OzarkRide claim that the number of users is similar throughout the four seasons. Is the average number of users different throughout the four seasons?_  

##### *Levine's Testing*
![Levene's Testing](\assets\images\One Way 1.PNG){: .align-center}   
>1. Levine’s Testing  
Ho: All variances are equal  
Ha: At least one variance is different  
2. α = 0.05  
3. F = 170.65  
4. p-value = 0.001 < 0.05 = α  
5. Reject the null hypothesis (proceed with caution)  
6. Conclusion: At least one variance is different.  

##### *One-Way ANOVA Testing*  
![One-Way Anova](\assets\images\One Way 2.PNG){: .align-center}  
>2. One-Way ANOVA Testing  
Ho: All means are equal  
Ha: At least one mean is different  
2. α = 0.05  
3. F = 409.18  
4. p-value = 0.001 < 0.05 = alpha  
5. Reject the null hypothesis  
6. There is a significant difference in the mean between seasons and the number of total users.  

##### *Tukey's Test*
![Tukey's Test](\assets\images\One Way 3 Tukey.PNG){: .align-center}  
The difference in the mean happens between Fall and Summer, Fall and Winter, and Fall and Spring. However, Summer and Winter means are pretty similar.  

##### One-Way ANOVA Take Away  
* There is no statistical evidence to support OzarkRide’s claim of having a similar number of users throughout the four seasons.  
* Running ANOVA makes sense in this case as we are trying to understand how the different seasons affect the number of users.  
* Tukey’s test helps us understand that the average mean users of the seasons are significantly different. We can support that by looking at Tukey’s graph where Fall – Summer, Fall -Winter, Fall – Spring is significantly different, and only Summer – Winter is similar. (Having *** means they are different)  

#### Regression Analysis  
_From all the operation data, can temperature, humidity, and windspeed predict the total users?_  
![Regression Analysis 1](\assets\images\Multiple Regression 1.PNG){: .align-center}    
>1. Overall model is significant  
2. temp is significant  
hum is significant  
windspeed is significant  
3. 
b0 = 178.81,
b1 = 362.53,
b2 = -273.46,
b3 = 26.32
4. Adj-R = 0.2512 or 25.12% of the variability can be explained by the total users, temperature, humidity, and windspeed.
5. Based on point #3:
    * If we hold all other independent variables constant, having one user increases the temperature by 362.53.
    * If we hold all other independent variables constant, having one user decreases the humidity by -273.46.
    * If we hold all other independent variables constant, having one user decreases the windspeed by 26.32.  

_From all the operation data, can temperature predict the number of casual users, and registered users?_  
![Regression Analysis 2](\assets\images\Multiple Regression 2.PNG){: .align-center}   
>1. Overall model is significant
2. casual is significant  
registered is significant  
3. b0 = 0.42,
b1 = 0.002,
b2 = 0.0002
4. Adj-R = 0.2253 or 22.53% of the variability can be explained by the temperature, casual users, and registered users.  
5. Based on point #3:
    * If we hold all other independent variables constant, increasing our temperature by 1 will increase casual users by 0.002.  
    * If we hold all other independent variables constant, increasing our temperature by 1 will increase registered users by 0.0002  

##### Multiple Linear Regression Take Away
* Two regression analyses were run just to make sure we were not missing anything. 
* On the first regression analysis, we can’t just add one user and have our temperature and windspeed increase while humidity decrease. It is not possible as we cannot control the weather.  
* In the second regression analysis, the result makes more sense because if the temperature increases there will be more people riding a bike. However, that is going to go until a certain point in the temperature. If the temperature is too high, it will be too hot outside, so people will not ride a bike. Therefore, as it gets warmer, more rental service will be used.  

-------------------------------------------------------------
### Files
[Raw Dataset](https://github.com/rfchungl/rchungl.github.io/blob/master/pages/Original%20Dataset.csv)  
[Working Dataset](https://github.com/rfchungl/rchungl.github.io/blob/master/pages/OzarkBike%20Analysis.xlsx)  
[SAS Exploration](https://github.com/rfchungl/rchungl.github.io/blob/master/pages/OzarkBike.egp)  
[OzarkRide Report Version](https://github.com/rfchungl/rchungl.github.io/blob/master/pages/OzarkRide.pdf)
