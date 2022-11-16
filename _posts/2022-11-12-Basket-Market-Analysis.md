---
layout: single
title: "Business Case: Basket Market Analysis"
excerpt: "Using Excel and Python to perform market basket/association analysis using dataset from a bakery."
output: html_document
tags: [python, excel, analysis]
comments: false
author_profile: true
toc: true
toc_sticky: true
---
### Business Scenario  
Ayala's Bakery is a family owned business with more than 30 years on the market delivering bakeries to its customers in Springdale, Arkansas. They updated their POS system a few years ago to keep track of their sales. However, they have not had success in discovering what products the customers like and what they usually like to pair with. Ayala's Bakery have approached Ruben to dive deep into the data to look for insights and provide recommendations.  

-----------------------------------------------------------------

### Written Memo
To: Ayala's Bakery Executives  
From: Ruben Chung, Data Analyst  
Subject: Ayala's Bakery Recommendations  
<br>
Based on the analyses performed, we found that the top 5 best-selling items are coffee, tea, cake, sandwich, and pastry. Coffee has by far the highest amount sold followed by tea. Interestingly, the time range where these two products are sold the most is between 10-1 pm for coffee and 2-3 pm for tea. Sandwich is best sold between 1-2 pm for lunchtime.  
<br>
Although coffee and tea are not at the top when it comes to margin, they are good to draw customers when paired with other products such as cake, pastry, and bread, thus, increasing the amount spent by customers. We found that the highest pairing combo is coffee and pastry. On the other hand, we don’t recommend pairing tea and bread. In addition, we recommend discontinuing the sale of muffins as it has the least sale and customers don’t like them.  
<br>
We recommend preparing enough coffee, tea, pastries, cakes, and sandwiches during the peak time using the time range to avoid stock out. Also, consider doing a marketing campaign that incentive customers to pair coffee or tea with any other products for a discount or loyalty points to increase Ayala’s Bakery revenue.  
<br>
If you have any questions, please contact me at rfchungl@uark.edu. Your voice matters.  
<br>
<br>
Sincerely,  
<br>
Ruben Chung

--------------------------------

### Analysis with Excel

![image](https://user-images.githubusercontent.com/115122030/196620687-b6f18dc6-cafb-4b10-a3ae-dd32ac6993d6.png)
First capture shows the top 5 items that are sold at Bread Basket. You can see that the undisputed top 1 is Coffee that almost quadruple the top 2 which is Tea.  
<br>
![image](https://user-images.githubusercontent.com/115122030/196620755-c2e2432b-c6c1-40f6-8bc8-47e0a611bb6c.png)  
Second capture shows the years where Bread Basket was on operation. We clearly see that during 2016, December has the highest number of transactions. This could mean that because the winter season is cold, people tend to go and buy hot beverage to get warm. On the other hand, March of 2017 has the highest number of transactions which triples the amount in transactions from December of 2016.  
<br>
![image](https://user-images.githubusercontent.com/115122030/196620794-97d09767-4256-42bb-83a1-33bfec3c641d.png)  
Third capture shows a list of items ranked by margin gain. We can see that the top 5 most sold items in capture 1 are not as profitable as the one shown in the capture above. Although coffee and tea are within top 2 most sold, extra salami or feta and tartine get more margin.  
<br>
![image](https://user-images.githubusercontent.com/115122030/196620841-bb6ec1df-4169-40d6-b64c-97354d8b4a4f.png)  
Last capture shows the time where the most sold items are bought. We can see that people tend to buy more tea between 2-3pm, cake between 2-3pm after lunch, sandwich between 1-2pm during lunch time, coffee between 10-1pm, and pastry between 10-11am. I thought that people would buy more coffee in the AM time but it seems that it’s very spread-out and the main time range are between 10-1pm.  

---------------------------------------------------------------
### Python Market Analysis
**Python:** [`Python Code Here`](https://github.com/rfchungl/Projects-Portfolio/blob/main/MarketBasketAnalysis/basket.py)  
<br>
**The Python code has comments on what does each line do for a better understanding of the program.**  
<br>
**Description:** Using Excel and Python to analyze the data transactions from a bakery to understand the results from the association analysis of the mentioned dataset. This project is supposed to be a business case, thus, the written memo at the end.  
<br>
**What does this program do?** It first read the csv file named "basket.csv" then it runs association analysis through a few lines of codes using the mlxtend.frequent_patterns apriori and association_rules libraries.  
<br>
**Things needed to run the Python code:**
```
- import pandas as pd
- from mlxtend.frequent_patterns import apriori
- from mlxtend.frequent_patterns import association_rules
```  
**Path to Bread Basket csv**
```
bread = pd.read_csv(r"/Users/yourusername/Desktop/basket.csv")
df = bread.groupby(['Transaction','Item']).size().reset_index(name='count')
breadbasket = (df.groupby(['Transaction', 'Item'])['count']
          .sum().unstack().reset_index().fillna(0)
          .set_index('Transaction'))
```  
**Transform values into dataframe**
```
def df(x):
    if x <= 0:
        return 0
    if x >= 1:
        return 1
dataset = breadbasket.applymap(df)
frequent_itemsets = apriori(dataset, min_support=0.01, use_colnames=True)
rules = association_rules(frequent_itemsets, metric="lift")
rules.sort_values('confidence', ascending = False, inplace = True)
rules.head()
print(rules)
```
----------------------------------

### Output
![basket1](https://user-images.githubusercontent.com/115122030/196619281-acf26716-1593-4d0f-9b60-6fb53aaff9b3.png)
![basket2](https://user-images.githubusercontent.com/115122030/196619284-b262992c-c730-44cc-aeec-4e673f52e71f.png)  
The association analysis using Python shows that the customer is 1.48 times more likely to buy toast then coffee than other customers.   

----------------------------------
### Python
[Python File Here](https://github.com/rfchungl/Projects-Portfolio/blob/main/MarketBasketAnalysis/basket.py)







