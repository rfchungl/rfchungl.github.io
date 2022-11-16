---
layout: single
title: "Building a Database and Visualization From Scratch"
description: "Dashboard made from scratch. It was condensed from a number of CSV files that were put into BigQuery. Afterwards, using BigQuery as the database to build the dashboard."
output: html_document
tags: [visualization, sql]
comments: false
author_profile: true
toc: true
toc_sticky: true
---
**Visualization:** [`Click Here For Visualization`](https://datastudio.google.com/reporting/2004c153-b0d4-42e1-8bee-1f3c6eaa2fa8)
<br><br>
**Note:** The dataset was modified in order to protect the privacy of the tutors and students. The date of the sample dataset is from 08-23-2021 to 10-23-2021.
<br><br>
**Description:** Condensing all weekly CSV files from tutoring into a database and use it to feed the dashboard.
<br>

--------------------------------------------------------------------------

### Business Problem 
Because the administrator in charge to maintain the booking systems at the university left, nobody was in charge to maintain it. The database would break by itself on days where a lot of web traffics happen. In order to prevent a disaster, the tutoring director decided to save the information of the booking appointments every week resulting in many CSV files. When there is a need to know something about tutoring, how many students came, what class was requested the most, etc, the director had to merge all the CSV files taking a lot of time just to get an answer. <br><br>
In order to reduce the time, I decided to condense all the CSV files into a database. In this case, I used free softwares to do that due to this being something that I purely wanted to do...not that I was required. Therefore, I chose the Google Analytics pack as the tools and used BigQuery to centralize all the booking informations and use DataStudio (Looker Studio) to provide visibility. Throughout my time working as a Graduate Assistant, I updated weekly the database once I was able to get to the CSV file.

-----------------------------------------------------------------------------------------------

### Procedure

I could have done it two ways: <br>
  1- Merge all the CSV files into Excel file and upload it to BigQuery <br>
  2- Upload weekly CSV file into BigQuery  
<br>
To show the full functionality of how I maintained a database, I am going to showcase method #2.

#### Part I

I created a bucket to save the CSV file digitally. Due to BigQuery being a web-based database, it cannot use your own desktop to find the path to the file you are going to upload.  
<br>
![bucket](https://user-images.githubusercontent.com/115122030/197105095-d1c834f0-5db3-46f6-8b57-dabd267ed68f.JPG)
I was able to start the database with three weeks of data sample. After that, every week when I got the CSV file from the director, I would upload it into the bucket.

#### Part II

Creating the database and setting up the schema.

![bigquery](https://user-images.githubusercontent.com/115122030/197105405-42d1f751-cdd6-42be-948f-881eaaa1f00e.JPG)
After creating the database in BigQuery, you can upload your first CSV file as the schema automatically or you can write by yourself.

#### Part III

In order to maintain it, I had to do a weekly BigQuery update to load the weekly report from the director.  

Example of data loading:  

##### Week #1

```
LOAD DATA INTO student-success-center-326917.Fall2021.datasetfall2021
FROM FILES (
  format = 'CSV',
  uris = ['gs://sscbucket/08-29-2021 to 09-04-2021.csv']);
```  

##### Week #2

```
LOAD DATA INTO student-success-center-326917.Fall2021.datasetfall2021
FROM FILES (
  format = 'CSV',
  uris = [' gs://sscbucket/09-05-2021 to 09-11-2021.csv']);
```

[`Click here for the rest of the queries`](https://github.com/rfchungl/Projects-Portfolio/blob/main/GoogleAnalytics/Load%20query.txt)


#### Part IV

Finally, after having the database, I used Google DataStudio now called *(Locker Studio)* to visualize what we have about booking appointments.

![database](https://user-images.githubusercontent.com/115122030/197105946-446a7fe3-8e9f-4916-a0fb-35874e92f74a.JPG)
When selecting the source of the data in DataStudio, select BigQuery as the database and choose the path of your dataset. In this case, I already created it on Part II.

--------------------------------------------------

### Final Result

![image](https://images.squarespace-cdn.com/content/v1/6301adc291fbaf2a00843f8f/976c7dfb-22d3-461c-8599-d67442d20e2f/fall2021.PNG?format=1000w)

--------------------------------------------------

### Conclusion
This way, I automated something that I basically had to pull every week for statistic purposes in a single dashboard where my supervisor would be able to check saving us hours of work.
