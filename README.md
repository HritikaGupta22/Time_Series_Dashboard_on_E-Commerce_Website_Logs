# Time_Series_Dashboard_on_E-Commerce_Website_Logs

## Methodology
The data for the E-Commerce Website Log was sourced from Kaggle(https://www.kaggle.com/datasets/kzmontage/e-commerce-website-logs/data). The dataset was first cleaned by removing spaces and then transformed into line protocol format. This formatted file was processed line-by-line, with each line being read at 8-second intervals and inserted into InfluxDB. A database sample was then created, along with a measurement named "churn." Finally, a dashboard was developed using Grafana.

## About Data
This dataset captures detailed logs from an e-commerce website, offering insights into user behavior and sales performance. It includes the time and duration of website access, IP addresses, and corresponding countries, allowing for geographic and temporal analysis. Information on the language and platform used provides insights into user preferences and device trends. Key metrics such as sales amounts, return amounts, and bytes consumed link user activity to revenue generation and data usage patterns. This dataset enables the creation of dashboards to analyze global reach, peak activity times, revenue trends, and platform optimization, making it a valuable resource for e-commerce analytics.

## Queries
****About Data****
- What is the Total Sales?
```
  select Sum("sales") from "churn";
```
- What is the Mean Returned Amount?
```
  select mean("returned_amount") from "churn";
```
- How much is the Average Bytes?
```
  select mean("bytes") from "churn";
```
****Sales and Revenue****
- What is the trend of sales over a time?
```
  SELECT "sales" FROM "churn" WHERE "time" >= now() - 1d;
```
- How do sales vary across different countries group by 1 hr?
```  
  SELECT SUM("sales") AS "Total Sales" INTO "COUNTRY_WISE_SALES" FROM "churn" WHERE time >= now() - 1d GROUP BY "country";
```
- What is the trend of sales by payment method (Credit Card, Debit Card, Cash) over 1 hours?
  ```
  SELECT SUM("sales") FROM "churn" GROUP BY time(1h),"pay_method";
  ```
- How do sales differ between male and female customers in last 1 hr?
```
  SELECT sum("sales") AS "Total Sales" FROM "churn" WHERE time >= now() - 1d GROUP BY "gender"
```
- How do sales from different membership types (Normal, Premium, etc.) vary in 5 minutes?
```
  SELECT SUM("sales")FROM "churn" GROUP BY time(1h), "membership";
  ```

****User Behavior Trends****
- How does the number of sessions vary across browsers (e.g., Chrome, Firefox) grouped every 10 minutes?
```
  SELECT count(“sales”) FROM "churn" GROUP BY time(1h),"accessed_Ffom";
```
- How does data usage (bytes) trend every 5 minutes, grouped by membership types (Normal, Premium)?
```
  SELECT sum("bytes") FROM "churn"GROUP BY time(1d),"membership";
```
- How does the total returned_amount trend in 10-minute intervals, filtered by payment method?
```
  SELECT sum("returned_amount") FROM "churn" GROUP BY time(1d), "pay_method";
```
****Returns and Refunds****
- What is the data consumption (bytes) trend over 10 minutes, grouped by country?
```
  SELECT sum("bytes") AS "Data Consumption (Bytes)" FROM "churn" GROUP BY time(1d),"country";
```
- How does sales performance fluctuate in 5-minute intervals for the top 5 countries by sales?
```
  SELECT "country" INTO top_countries FROM "churn" GROUP BY "country" ORDER BY sum("sales") DESC LIMIT 5)
  SELECT sum("sales") FROM "top_countries" GROUP BY time(1h),"country"
```
****Prediction of Sales****
- What will be the sales for the Premium Membership and TCP network Protocol?
```
  SELECT moving_average("sales", 3) FROM "churn" WHERE "membership" = 'Premium' AND "network_protocol" = 'TCP' and time > now() -1h;
```
## Dashboard

[![E-Commerce Website Log Data](![image (1)](https://github.com/user-attachments/assets/359a620c-d702-4eab-b3eb-205a887efce5)
)](https://www.youtube.com/watch?v=KEeDcjHAKhg)

## Interpretations
**General Insights**
- The gauge shows total sales over the last day, amounting to **250,438 units**. This reflects the overall revenue-driving metric and is useful for monitoring the performance during the specified period. The high value indicates strong transactional activity, but trends over time can provide deeper insights.
- The mean returned amount is **34.0**, indicating that on average, customers return items worth this amount. This can signify issues with product quality, mismatched expectations, or other factors leading to returns. Monitoring this metric over time can reveal customer satisfaction trends.
- The **1,684 average bytes** metric represents average data usage per session. This provides insights into user activity levels on the platform, potentially indicating engagement trends or issues with high data consumption for slower connections.
  
**Sales and Revenue Analysis**  
- The gauge shows total sales over the last day, amounting to *250,438 units*. This reflects the overall revenue-driving metric and is useful for monitoring the performance during the specified period. The high value indicates strong transactional activity, but trends over time can provide deeper insights.  
- The line graph shows sales activity over 24 hours, with noticeable spikes around *evening hours (18:00 - 21:00)*. This suggests higher customer activity during these hours, likely due to after-work shopping. The graph can help optimize promotional campaigns during peak times.  
- India (IN) overwhelmingly dominates sales performance, with *91,138 total sales*, while other countries like Russia, Peru, and Germany contribute marginally. This highlights India's key market position and suggests a potential need to boost international marketing efforts.  
- Premium members consistently drive higher sales compared to Normal members over hourly intervals. This validates the importance of premium memberships in contributing to revenue and suggests opportunities for further incentivizing upgrades.  
- Female customers contribute *58%* of total sales, while males account for *42%*. This indicates a slightly higher spending pattern among females, which could inform product targeting and marketing campaigns.  

**User Behavior Trends**  
- The *1,684 average bytes* metric represents average data usage per session. This provides insights into user activity levels on the platform, potentially indicating engagement trends or issues with high data consumption for slower connections.  
- Premium members consume more data than normal members, indicating higher engagement or activity levels. The trend also shows a steady decline in data usage after peak activity hours, suggesting users' habits.  
- The graph compares session counts across various browsers. *Android App sessions dominate*, indicating mobile app popularity, while other platforms have lower engagement. Insights from this graph could direct resources to further optimize the mobile app experience.  

**Returns and Refunds** 
- The mean returned amount is *34.0*, indicating that on average, customers return items worth this amount. This can signify issues with product quality, mismatched expectations, or other factors leading to returns. Monitoring this metric over time can reveal customer satisfaction trends.  
- Returns filtered by payment methods show that *Cash* and *Credit Card* have higher returned amounts, while Debit Card and Other methods have relatively fewer returns. This could indicate differences in customer confidence or fraud detection in certain payment methods.  

**Prediction of Sales** 
- Predicted sales for premium members and TCP network protocol show a value of *34.7*, suggesting consistent engagement in this segment. This metric can guide future infrastructure planning and promotional strategies for premium memberships.
