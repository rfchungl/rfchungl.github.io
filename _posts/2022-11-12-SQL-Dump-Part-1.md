---
layout: single
title: "SQL Dump - Part 1"
excerpt: "Introductory SQL statements with outputs"
output: html_document
tags: [sql]
comments: false
author_profile: true
toc: true
toc_sticky: true
---
{{ page.excerpt }}

### Description
A mix of SQL problems that involves aggregate functions and joins. 

---------------------------
### Problems
**1.What are the total sales for the second quarter of 2013?**
```
SELECT SUM(Quantity * Unit_Price) AS Total_Sales  
FROM A2C_Order_Fact O  
JOIN A2C_Date_Dimension DD ON O.Date_Key = DD.Date_Key  
WHERE Quarter_of_Year = 2 AND Year_Number = 2013  
GROUP BY Quarter_of_Year;  
 ```
**2.What are the total sales for albums in the second quarter of 2013?**
```
SELECT SUM(Quantity * Unit_Price) AS Album_Total_Sales  
FROM A2C_Order_Fact O  
	JOIN A2C_Date_Dimension DD ON O.Date_Key = DD.Date_Key  
	JOIN A2C_Item_Dimension ID ON O.Item_Key = ID.Item_Key  
WHERE Quarter_of_Year = 2 AND Item_Type = 'Album' AND Year_Number = 2013;  
```
**3.What are the total sales of albums, bought by female customers living in Arizona and California?**
```
SELECT SUM(Quantity * Unit_Price) AS Female_Album_Sales  
FROM A2C_Order_Fact O  
	JOIN A2C_Customer_Dimension CD ON O.Customer_Key = CD.Customer_Key  
	JOIN A2C_Item_Dimension ID ON O.Item_Key = ID.Item_Key  
WHERE CD.Gender = 'F' AND CD.State_Name IN ('Arizona', 'California') AND Item_Type = 'Album';  
```
**4.Who is the best agent, according to the sales data for 2014?**
```
SELECT TOP 1 SUM(Quantity * Unit_Price) AS Total_Sales, Agent_Name  
FROM A2C_Order_Fact O  
	JOIN A2C_Date_Dimension DD on O.Date_Key = DD.Date_Key  
	JOIN A2C_Item_Dimension ID on O.Item_Key = ID.Item_Key  
WHERE Year_Number = 2014  
GROUP BY Agent_Name  
ORDER BY Total_Sales DESC;  
```
**5.What are the total sales from each customer each year?**
```
SELECT Customer_Name, SUM(Quantity * Unit_Price) AS Total_Sales, Year_Number  
FROM A2C_Order_Fact O  
	JOIN A2C_Customer_Dimension CD ON O.Customer_Key = CD.Customer_Key  
	JOIN A2C_Date_Dimension DD ON O.Date_Key = DD.Date_Key  
GROUP BY Customer_Name, Year_Number  
ORDER BY Customer_Name, Total_Sales DESC;  
```
**6.How much revenue each state generates by year?**
```
SELECT State_Name, Year_Number, SUM(Quantity * Unit_Price) AS Total_Sales  
from A2C_Order_Fact O  
	JOIN A2C_Customer_Dimension CD ON O.Customer_Key = CD.Customer_Key  
	JOIN A2C_Date_Dimension DD ON O.Date_Key = DD.Date_Key  
GROUP BY State_Name, Year_Number  
ORDER BY Total_Sales DESC;  
```
 
