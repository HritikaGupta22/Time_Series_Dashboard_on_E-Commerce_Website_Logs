# Time_Series_Dashboard_on_E-Commerce_Website_Logs

## Methodology
The data for the E-Commerce Website Log was sourced from Kaggle. The dataset was first cleaned by removing spaces and then transformed into line protocol format. This formatted file was processed line-by-line, with each line being read at 8-second intervals and inserted into InfluxDB. A database sample was then created, along with a measurement named "churn." Finally, a dashboard was developed using Grafana.

## About Data
This dataset captures detailed logs from an e-commerce website, offering insights into user behavior and sales performance. It includes the time and duration of website access, IP addresses, and corresponding countries, allowing for geographic and temporal analysis. Information on the language and platform used provides insights into user preferences and device trends. Key metrics such as sales amounts, return amounts, and bytes consumed link user activity to revenue generation and data usage patterns. This dataset enables the creation of dashboards to analyze global reach, peak activity times, revenue trends, and platform optimization, making it a valuable resource for e-commerce analytics.

## Queries
****Sales and Revenue****
- What is the trend of sales over a time?

  SELECT "sales" FROM "churn" WHERE "time" >= now() - 1d;
- How do sales vary across different countries group by 1 hr?
  
  SELECT SUM("sales") AS "Total Sales" INTO "COUNTRY_WISE_SALES" FROM "churn" WHERE time >= now() - 1d GROUP BY "country";
- What is the trend of sales by payment method (Credit Card, Debit Card, Cash) over 1 hours?
  
  SELECT SUM("sales") FROM "churn" GROUP BY time(1h),"pay_method";
- How do sales differ between male and female customers in last 1 hr?

  SELECT sum("sales") AS "Total Sales" FROM "churn" WHERE time >= now() - 1d GROUP BY "gender"
- How do sales from different membership types (Normal, Premium, etc.) vary in 5 minutes?

  SELECT SUM("sales")FROM "churn" GROUP BY time(1h), "membership";
  
****User Behavior Trends****
- How does the number of sessions vary across browsers (e.g., Chrome, Firefox) grouped every 10 minutes?

  SELECT count(“sales”) FROM "churn" GROUP BY time(1h),"accessed_Ffom";
- How does data usage (bytes) trend every 5 minutes, grouped by membership types (Normal, Premium)?

  SELECT sum("bytes") FROM "churn"GROUP BY time(1d),"membership";
- How does the total returned_amount trend in 10-minute intervals, filtered by payment method?

  SELECT sum("returned_amount") FROM "churn" GROUP BY time(1d), "pay_method";

****Returns and Refunds****
- What is the data consumption (bytes) trend over 10 minutes, grouped by country?

  SELECT sum("bytes") AS "Data Consumption (Bytes)" FROM "churn" GROUP BY time(1d),"country";
- How does sales performance fluctuate in 5-minute intervals for the top 5 countries by sales?

  SELECT "country" INTO top_countries FROM "churn" GROUP BY "country" ORDER BY sum("sales") DESC LIMIT 5)
  SELECT sum("sales") FROM "top_countries" GROUP BY time(1h),"country"

****Prediction of Sales****
- What will be the sales for the Premium Membership and TCP network Protocol?

  SELECT moving_average("sales", 3) FROM "churn" WHERE "membership" = 'Premium' AND "network_protocol" = 'TCP' and time > now() -1h;

## Dashboard


## Interpretations

## Interpretations
