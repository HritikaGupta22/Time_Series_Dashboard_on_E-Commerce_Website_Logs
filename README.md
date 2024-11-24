# Time_Series_Dashboard_on_E-Commerce_Website_Logs

## Methodology
The data for the E-Commerce Website Log was sourced from Kaggle. The dataset was first cleaned by removing spaces and then transformed into line protocol format. This formatted file was processed line-by-line, with each line being read at 8-second intervals and inserted into InfluxDB. A database sample was then created, along with a measurement named "churn." Finally, a dashboard was developed using Grafana.

## About Data
This dataset captures detailed logs from an e-commerce website, offering insights into user behavior and sales performance. It includes the time and duration of website access, IP addresses, and corresponding countries, allowing for geographic and temporal analysis. Information on the language and platform used provides insights into user preferences and device trends. Key metrics such as sales amounts, return amounts, and bytes consumed link user activity to revenue generation and data usage patterns. This dataset enables the creation of dashboards to analyze global reach, peak activity times, revenue trends, and platform optimization, making it a valuable resource for e-commerce analytics.

## Queries
****Sales and Revenue Analysis****
What are the total sales in every 5 minutes?
How do sales vary across different countries group by 10 min?
What is the trend of sales by payment method (Credit Card, Debit Card, Cash) over 10 min?
How do sales differ between male and female customers in last 1 hr?
How do sales from different membership types (Normal, Premium, etc.) vary in 5 minutes?

****User Behavior Trends****
What is the total number of sessions (grouped by duration_(secs)) in 5-minute intervals?
How does the number of sessions vary across browsers (e.g., Chrome, Firefox) grouped every 10 minutes?
What is the hourly trend of sessions grouped by gender (Male/Female)?
How does data usage (bytes) trend every 5 minutes, grouped by membership types (Normal, Premium)?


****Returns and Refunds****
What is the number of returned transactions (Yes/No) in the last 30 minutes, grouped by country?
How does the total returned_amount trend in 10-minute intervals, filtered by payment method?
What is the hourly trend of returns (Yes/No) by gender?
How do returned transactions in the last 1 hour vary by browser type (e.g., Chrome, Firefox)?


****Geographic Analysis****
What is the data consumption (bytes) trend over 10 minutes, grouped by country?
How does sales performance fluctuate in 5-minute intervals for the top 5 countries by sales?

****Performance Metrics****
What is the correlation between session duration (duration_(secs)) and sales in the last 30 
minutes?


## Dashboard


## Interpretations
