
# Telco Offer, Load & Transaction Analysis

<p align="center">
  <img src="https://github.com/NISSAN40499/Telco-offer-load-transaction-Data-Analysis/blob/main/image/image_2025-10-19_171822907.png" width="550">
</p>

---
## Project Overview
This project focuses on analyzing Telco offer, load, and transaction data to generate actionable insights on product sales, revenue, customer behavior, and operational performance. Using SQL and BigQuery, the project identifies top-selling products by bundle type, calculates total revenue and average selling prices, evaluates cancellation and complaint rates, and tracks customer purchase behavior across divisions. Advanced SQL techniques such as CTEs and window functions are implemented to optimize query performance and analytical depth.

**Queries**
[View the full SQL code here](https://github.com/NISSAN40499/Telco-offer-load-transaction-Data-Analysis/blob/main/telco_code_sql.pdf/Code.md)


## Technologies Used

* **Backend / Analysis:** SQL, BigQuery
* **Data Handling & Storage:** Google BigQuery Dataset
* **Reporting / Visualization:** Optional export to Excel or Power BI

## Why These Technologies?

* **SQL & BigQuery**: Efficiently handles large-scale telecom transaction data (~50k+ records) with complex analytical queries.
* **CTEs & Window Functions**: Allow ranking, partitioning, and advanced aggregations to extract meaningful insights.
* **Optional Excel/Power BI**: For visualization, dashboards, and reporting for non-technical stakeholders.

## Project Structure

```
Telco-SQL-Analysis/
│
├── image
├── License
├── README.md
├── telco.pdf
├── telco_code_sql               
├── 01_Which Product is Most Ordered Per Bundle Type?
├── 02_Which Bundle Type is Least Ordered in Each Division?
├── 03_How Many Products are There per Bundle Type?
├── 04_Total Revenue and Average Selling Price per Product, Bundle Type, Operator, and Division
├── 05_Product with Highest Number of Sales & Total Revenue
├── 06_Product & Bundle Type Contributing Highest Percentage of Total Revenue
├── 07_Maximum & Minimum Discount and Commission per Product
├── 08_Operator & Division with Highest Avg Discount Across Products
├── 09_Unique Customers and Repeat Buyers
├── 10_Customer with Highest Orders / Total Spending
├── 11_Average Purchase Frequency of Customers by Bundle Type
├── 12_Most Orders by Hour of the Day
├── 13_Average Fulfillment / Delivery Time per Operator and Division
├── 14_Average Delivery Time per Division
├── 15_Cancellation Rate & Complaint Rate per Operator & Division
├── 16_Most Common Complaint Reasons
├── 17_Total Orders per Division
├── 18_Division with Highest Sales & Transactions
├── 19_Order Status Percentage per Operator & Division
├── 20_Products Offered by Each Operator
├── 21_Product Popularity by Validity Period
├── 22_Advanced Product Popularity using CTE & Window Function
├── 23_Products with Most Complaints & Cancellations
├── 24_Potential Revenue Loss Due to Cancellation
└── 25_Top 3 or 5 Most-Selling Products/Bundles in Key Divisions
```

Each SQL file contains queries for a specific analysis objective, structured for readability and modular execution.

## Features

* Identify most and least ordered products per bundle type and division.
* Calculate total revenue, average selling price, and product contribution to overall revenue.
* Track maximum and minimum discounts and commissions per product.
* Analyze customer behavior: purchase frequency, repeat buyers, and top spenders.
* Evaluate operational efficiency: order fulfillment times, peak order hours.
* Assess service quality: cancellation and complaint rates, common complaint reasons.
* Determine product popularity by validity period and top-selling products in key divisions.

## Challenges Solved

* Handling multiple analytical queries over large datasets efficiently using SQL and BigQuery.
* Ranking and partitioning data to identify top products per bundle type/division.
* Calculating advanced metrics such as potential revenue loss, average purchase frequency, and percentage contribution of products to total revenue.
* Structuring queries for readability and modular execution for easy future expansion.

## Future Improvements

* Integrate with **Power BI or Tableau dashboards** for interactive visualization and reporting.
* Automate daily/weekly updates to track dynamic trends in real-time.
* Include predictive analytics to forecast sales and customer demand per bundle type.
* Expand dataset with **customer demographics** to improve segmentation and personalized offers.
* Build a **web-based reporting portal** for stakeholders to access insights on-demand.

---

Thanks for reading...
