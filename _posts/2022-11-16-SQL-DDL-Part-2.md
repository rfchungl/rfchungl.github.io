---
layout: single
title: "SQL Data Definition Language - Part 2"
output: html_document
tags: [sql]
comments: false
author_profile: true
toc: true
toc_sticky: true
excerpt: "Intermediate to using SQL data definition language to create tables with relationships."
---
### Description
Intermediate problems to data definition language and different ways and scenarios to apply them using create and alter functions.

---------------------------
### Problems

#### 1. Dimensional Data Model & Data Dictionary Specification -> DDL  
Our DW consultant gave us a good starting point for a simple “sales” data mart design. Create the tables and relationships based on the following data model and data dictionary.   

![SQL1](\assets\images\sql4\1.png){: .align-center} 
```
CREATE TABLE A2B_Item_Dim  (
    Item_Key  int NOT NULL,
    Item_Id  int NOT NULL,
    Item_Name varchar(50) NOT NULL,
    Item_Type varchar(10) NOT NULL,
    Band_Name varchar(50) NOT NULL,
    Release_Date DATE   NOT NULL,
    Agent_Name varchar(40) NOT NULL,
    PRIMARY KEY (Item_Key)
);
CREATE TABLE A2B_Date_Dim  (
    Date_Key   int NOT NULL,
    Date_Desc  DATE NOT NULL,
    Day_of_Month int NOT NULL,
    Month_of_Year int NOT NULL,
    Year_Number int NOT NULL,
    Day_of_Year int NOT NULL,
    Quarter_of_Year int NOT NULL,
    Day_of_Week varchar(15) NOT NULL,
    PRIMARY KEY (Date_Key)
);
CREATE TABLE A2B_Customer_Dim (
    Customer_Key   int NOT NULL,
    Customer_Id  int NOT NULL,
    Customer_Name varchar(50) NOT NULL,
    Street_Address varchar(50) NOT NULL,
    Zip_Code char(5) NOT NULL,
    City varchar(20) NOT NULL,
    State_Name varchar(15) NOT NULL,
    Gender varchar(1) NOT NULL,
    DOB DATE   NOT NULL,
    SC_Begin_Date DATE   NOT NULL,
    SC_End_Date DATE   NOT NULL,
    PRIMARY KEY (Customer_Key)
);
CREATE TABLE A2B_Order_Fact   (
    Item_Key  int NOT NULL,
    Customer_Key  int NOT NULL,
    Date_Key  int NOT NULL,
    Order_Id  int NOT NULL,
    Quantity  int NOT NULL,
    Unit_Price  DECIMAL(6,2) NOT NULL,
    FOREIGN KEY (Item_Key) REFERENCES A2B_Item_Dim(Item_Key),
    FOREIGN KEY (Customer_Key) REFERENCES A2B_Customer_Dim(Customer_Key),
    FOREIGN KEY (Date_Key) REFERENCES A2B_Date_Dim(Date_Key)
);
```

---------

#### 2. External Data Source & Data Mart Extension -> DDL  
Hallux has recently started receiving enhanced Billboard® charting information for the songs in their catalog, including weekly rank, previous week rank, number of downloads, iTunes sales and Spotify streams. We need to be able to take this data and merge it with our existing warehouse tables to allow us to perform analysis over time on this new data stream. Include fields for our internal weekly sales (in units and $) to store alongside this data. You can assume that the ETL job that will be loading this data can locate the correct item_key that corresponds with the Song data coming from Billboard (hint: conformed dimension). We need to make sure that we have the appropriate units of time in a dimension such as week of the year, month of the year, month description, season, and a flag for “festival season” (which stretches from late Spring to early Fall).   
Here’s a snippet of the csv file coming from Billboard:    

![SQL2](\assets\images\sql4\2.PNG){: .align-center}  
```
CREATE TABLE A2B_Band_Dim (
Band_ID int NOT NULL,
Band_Name varchar(50) NOT NULL,
PRIMARY KEY (Band_ID)
);
CREATE TABLE A2B_Song_Dim (
Song_ID int NOT NULL,
Item_Key int NOT NULL,
Song_Name varchar(40) NOT NULL,
PRIMARY KEY (Song_ID),
FOREIGN KEY (Item_Key) REFERENCES A2B_Item_Dim(Item_Key)
);
CREATE TABLE A2B_Sales_Fact (
Sales_ID int NOT NULL,
Previous_rank int,
Current_rank int,
Downloads int NOT NULL,
iTunes_Sales decimal(10,2) NOT NULL,
Spotify_Streams int NOT NULL,
Date_Key int NOT Null,
Item_Key int NOT NULL,
Band_ID int NOT NULL,
PRIMARY KEY (Sales_ID),
FOREIGN KEY (Item_Key) REFERENCES A2B_Item_Dim(Item_Key),
FOREIGN KEY (Date_Key) REFERENCES A2B_Date_Dim(Date_Key),
FOREIGN KEY (Band_ID) REFERENCES A2B_Band_Dim(Band_ID)
);
```

-----

#### 3. Extending the DW from a System of Record (SoR) -> DDL  
Management has asked that we add performance metrics to the DW. Using the data in our existing transaction system, we need to construct a star schema that allows us to track the revenue for each performance by venue, date, band and booking agent. We will need important information about the venue (including name, contact information, and location details), the band (including name, formation date, genres, and contact information) and the booking agent (at this point we’re only concerned about the agent’s name). For bands, consider how you need to represent that the band might be in multiple genres, capturing at least a primary and secondary genre. For venues, they sometimes move across town or change their name under new ownership, so design to track such changes. Where possible, use conformed dimensions (hint: likely only one in this case). Here are the relevant tables from the SoR.    

![SQL3](\assets\images\sql4\3.png){: .align-center} 
```
ALTER TABLE A2B_Band_Dim 
ADD Contact_Info varchar(15), Formation_Date datetime

CREATE TABLE A2B_Genre_Dim (
Genre_ID int NOT NULL,
Band_ID int NOT NULL,
Genre1 varchar(20) NOT NULL,
Genre2 varchar(20) NOT NULL,
PRIMARY KEY (Genre_ID),
FOREIGN KEY (Band_ID) REFERENCES A2B_Band_Dim(Band_ID)
);
CREATE TABLE A2B_Booking_Agent_Dim (
Genre_ID int NOT NULL,
Agent_ID int NOT NULL,
Band_ID int NOT NULL,
Agent_Name varchar(50) NOT NULL,
PRIMARY KEY (Agent_ID),
FOREIGN KEY (Band_ID) REFERENCES A2B_Band_Dim(Band_ID)
);
CREATE TABLE A2B_Venue_Dim (
Venue_ID int NOT NULL,
Venue_Name varchar(40) NOT NULL,
Venue_Contact_Info varchar(15),
Location_Details varchar(50),
PRIMARY KEY (Venue_ID)
);
CREATE TABLE A2B_Performance_Dim (
Performance_ID int NOT NULL,
Band_ID int NOT NULL,
Venue_ID int NOT NULL,
Agent_ID int NOT NULL,
Performance_Date datetime,
PRIMARY KEY (Performance_ID),
FOREIGN KEY (Band_ID) REFERENCES A2B_Band_Dim(Band_ID),
FOREIGN KEY (Venue_ID) REFERENCES A2B_Venue_Dim(Venue_ID),
FOREIGN KEY (Agent_ID) REFERENCES A2B_Booking_Agent_Dim(Agent_ID)
);
CREATE TABLE A2B_Revenue_Fact (
Revenue_ID int NOT NULL,
Revenue decimal(10,2) NOT NULL,
Performance_ID int NOT NULL,
Band_ID int NOT NULL,
Venue_ID int NOT NULL,
Agent_ID int NOT NULL,
FOREIGN KEY (Band_ID) REFERENCES A2B_Band_Dim(Band_ID),
FOREIGN KEY (Venue_ID) REFERENCES A2B_Venue_Dim(Venue_ID),
FOREIGN KEY (Agent_ID) REFERENCES A2B_Booking_Agent_Dim(Agent_ID),
FOREIGN KEY (Performance_ID) REFERENCES A2B_Performance_Dim(Performance_ID)
);
```


