## Introduction:
The DataFrame contains the information about customer's purchases across United Kingdom. There are nearly about 4000 customers and their purchases during the period of one year (from 2010-12-01 to 2011-12-09). The dataset was downloaded from Kaggle: https://www.kaggle.com/datasets/ilkeryildiz/online-retail-listing/data

## Objective:
We need to amplify the efficiency of marketing strategies and boost sales through *customer segmentation*. We aim to transform the transactional data into a customer-centric dateset by creating new features that will facilitate the segmentation of customers into distinct groups using the k-means clustering algorithm. This segmentation will allow us to understand the distinct profiles and preferences of different customer groups. We then intend to develop a recommendation system that will suggest top-selling products to customers within each segment who haven't purchased those items yet, ultimately enhancing marketing efficiency and increasing sales.
## Action:
In order to complete this project, I divide it into 6 different parts:
1. Data Importing and Data Cleaning.
2. EDA
3. Cohort Analysis of customers based on Time Cohorts.
4. Recency, Frequency and Monetary Value Analysis (RFM).
5. Customer Segmentation using K-Means Algorithm: Segment customers into distinct groups using K-means, facilitating targeted marketing and personalized strategies.
6. Recommendation System: Implement a system to recommend best-selling products to customers within the same cluster who haven't purchased those products, aiming to boost sales and marketing effectiveness.

### Part 1: Data Cleaning
1. Check duplicates, missing values and anomalies;
2. Look into anomalies (This is to ensure data consistency and uncover hidden stories). For example, as `StockCodes`,`Quantity`,`UnitPrice` and `Description` are directly related to the prodcuts customers purchase. We can't just remove outliers straight away but investigate them. This approach helped uncover the reasons behind unusual data points. This deeper understanding was valuable for improving future data quality.

### Part 2: Data Exploration
This part is mainly focused on gaining a comprehensive picture of sales, including top10 products sold, sales trends over a year, month and day, average expenses by customers.
<img width="792" alt="Screenshot 2023-11-22 at 19 51 56" src="https://github.com/Lexi888/Customer_Behavior_Analysis/assets/98598719/641a03be-9999-4429-938d-09f805c8dbba">

The bar chart on the left indicates products were most purchased by customers, but the bar char on the right indicates the top 10 sales.

<img width="782" alt="Screenshot 2023-11-22 at 19 52 25" src="https://github.com/Lexi888/Customer_Behavior_Analysis/assets/98598719/81044390-c778-4679-b5a6-35c4f5ef201e">

<img width="781" alt="Screenshot 2023-11-22 at 19 53 33" src="https://github.com/Lexi888/Customer_Behavior_Analysis/assets/98598719/3b09bf0b-5317-487f-b8a7-b8b2fc41e889">

The above graph shows that Sales Trend from Dec 2010 to Dec 2011. The peak occured in November 2011, which could be contributed to preparation of Christmas or special sales.

<img width="770" alt="Screenshot 2023-11-22 at 19 54 40" src="https://github.com/Lexi888/Customer_Behavior_Analysis/assets/98598719/e6730f0d-5f8c-444e-8143-74ea40e4207b">

The above graph shows that most customers shopped between 10am to 3pm, so marketing campaign could focus on this time period to promote products.

### Part 3: Cohort Analysis
This method is to track and study user engagement over time. A cohort is a group of users who perform a certain sequence of events within a particular timeframe.

*Cohort analysis* is useful to identify trends within customer behavior that may be hidden when looking at more general analytics data. For example, overall analytics data may indicate an increasing number of monthly purchases, a very positive sign for the business. However, cohort analysis can reveal that, in fact, **the higher overall percentage is due to many first-time buyers**, while cohorts of older customers are actually returning to make purchases much less frequently than in the past. Thus, by following the behavior of particular cohorts over time, a more accurate view of business performance is possible.

To identify the **first-time buyers**, I created a new column `CohortMonth`. To calculate the retaintion rate (the percentage of users who continue using your product or service over a given time period), I needed to calculate `CohortPeriod`, which was equal to the balance between the time of latest buy minus the time of first buy. 

<img width="773" alt="Screenshot 2023-11-22 at 20 18 01" src="https://github.com/Lexi888/Customer_Behavior_Analysis/assets/98598719/7a20bd0a-4103-4c88-842c-d4aa7191a712">

From the above image, it indicated retention rate per month. The first column indicated the number of consumers who first shopped here, and the rest of columns showed the percentage of them continued coming back.

### Part 4: FRM Analysis
RFM is a method used for analyzing customer value and segmenting the customer base.
1. Recency (R): This metric indicates how recently a customer has made a purchase. A lower recency value means the customer has purchased more recently, indicating higher engagement with the brand.
2. Frequency (F): This metric signifies how often a customer makes a purchase within a certain period. A higher frequency value indicates a customer who interacts with the business more often, suggesting higher loyalty or satisfaction.
3. Monetary (M): This metric represents the total amount of money a customer has spent over a certain period. Customers who have a higher monetary value have contributed more to the business, indicating their potential high lifetime value.
Together, these metrics help in understanding a customer's buying behavior and preferences, which is pivotal in personalizing marketing strategies and creating a recommendation system.
**The RFM Analysis helps identify:**
- Who are your best customers?
- Which of your customers could contribute to your churn rate?
- Who has the potential to become valuable customers?
- Which of your customers can be retained?
- Which of your customers are most likely to respond to marketing campaign?

#### Rank Customers based on FRM:
<img width="474" alt="Screenshot 2023-11-22 at 20 27 36" src="https://github.com/Lexi888/Customer_Behavior_Analysis/assets/98598719/b3d30629-1db4-4799-a336-40c695f29b62">

From the example above, we can see that Customer12346 only shopped once and it's been 326 days since the first buy, so its R_rank and F_rank were much lower compared with other four customers. However, I noticed that RFM were on different ranges, so **normalization** was needed. The result was shown below:

<img width="651" alt="Screenshot 2023-11-22 at 21 10 52" src="https://github.com/Lexi888/Customer_Behavior_Analysis/assets/98598719/479b90d5-497b-497b-b045-a109fcfd9b36">

Now we can see that FRM is on the same range from 0-100.

Here is another problem we need to consider about: How to measure FRM Scores? What's the percentage should be given to each metric? - To answer this question, we need to consider the nature of business.

For example, in a consumer durable business, the **Monetary value** per transaction is normally high but **frequency** and **recency** are low. In this case, marketers should give more weight to monetary and recency rather than frequency.

If it's a retail business or online app, marketers should give more weight to **recency** and **frequency** than **monatary**.
Based on the case we work on here, the average unitprice is 3.47, so we can exclude it's a consumer durable business, therefore, we put more weight on recency and frequency.

*RFM Score is calculated based on Recency, Frequency, Monetary value normalize ranks. Based upon this score, we divide our customers. Here we rate them on a scale of 5.*

Formula used for calculating RFM Score is: *0.35 * R_score + 0.35 * F_score + 0.3 * M_score*

<img width="205" alt="Screenshot 2023-11-22 at 21 16 08" src="https://github.com/Lexi888/Customer_Behavior_Analysis/assets/98598719/921933ad-339d-4b36-9ae1-ed61d87c0bdd">

Now we need a threshold to measure FRM Score. To do this, I chose to get a descriptive summary of RFM Score, including max, min and mean. Based on the threshold, my standard were as below:
- RFM >= 3.5 : Top Customer
- 3.5 > RFM >= 2.5 : High Value Customer
- 2.5 > RFM >=2 : Medium value customer
- 1.39 >= RFM >1.2 : Low-value customer
- RFM <= 1.2 : Lost Customer

<img width="542" alt="Screenshot 2023-11-22 at 21 21 10" src="https://github.com/Lexi888/Customer_Behavior_Analysis/assets/98598719/83729267-3f05-4ef3-a81b-e632242e1870">

### Observations:
The Top Customers accounts for 27%, which can be referred as loyal customers. Business should target more on High value customers and Medium Value customers, to ensure they will become loyal customers. For Low value customers, company could try to increase their value by offering them incentives, discounts or personalized recommendations. And for lost customers, company needed to investigate why this would happen to reduce churn rates.
