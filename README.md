
# Telco Offer, Load & Transaction Analysis

<p align="center">
  <img src="https://github.com/NISSAN40499/Sales-Intelligence-SQL-Project-From-Trends-to-Segments/blob/main/images/Mysql%20image.jpg" width="450">
</p>


**Project Overview**
This project focuses on analyzing Telco offer, load, and transaction data to generate actionable insights on product sales, revenue, customer behavior, and operational performance. Using SQL and BigQuery, the project identifies top-selling products by bundle type, calculates total revenue and average selling prices, evaluates cancellation and complaint rates, and tracks customer purchase behavior across divisions. Advanced SQL techniques such as CTEs and window functions are implemented to optimize query performance and analytical depth.

**Live Demo**
[View Analysis in BigQuery](#)  *(replace with actual link if available)*

**Technologies Used**

* **Backend / Analysis:** SQL, BigQuery
* **Data Handling & Storage:** Google BigQuery Dataset
* **Reporting / Visualization:** Optional export to Excel or Power BI

**Why These Technologies?**

* **SQL & BigQuery**: Efficiently handles large-scale telecom transaction data (~50k+ records) with complex analytical queries.
* **CTEs & Window Functions**: Allow ranking, partitioning, and advanced aggregations to extract meaningful insights.
* **Optional Excel/Power BI**: For visualization, dashboards, and reporting for non-technical stakeholders.

**Project Structure**

```
Telco-SQL-Analysis/
│
├── README.md                # Project documentation
├── 01_most_ordered_products.sql
├── 02_least_ordered_bundles.sql
├── 03_total_revenue_avg_price.sql
├── 04_highest_revenue_product.sql
├── 05_max_min_discount_commission.sql
├── 06_customer_analysis.sql
├── 07_delivery_and_order_time.sql
├── 08_cancellation_complaint_analysis.sql
├── 09_product_popularity.sql
└── 10_top_products_by_division.sql
```

Each SQL file contains queries for a specific analysis objective, structured for readability and modular execution.

**Features**

* Identify most and least ordered products per bundle type and division.
* Calculate total revenue, average selling price, and product contribution to overall revenue.
* Track maximum and minimum discounts and commissions per product.
* Analyze customer behavior: purchase frequency, repeat buyers, and top spenders.
* Evaluate operational efficiency: order fulfillment times, peak order hours.
* Assess service quality: cancellation and complaint rates, common complaint reasons.
* Determine product popularity by validity period and top-selling products in key divisions.

**Challenges Solved**

* Handling multiple analytical queries over large datasets efficiently using SQL and BigQuery.
* Ranking and partitioning data to identify top products per bundle type/division.
* Calculating advanced metrics such as potential revenue loss, average purchase frequency, and percentage contribution of products to total revenue.
* Structuring queries for readability and modular execution for easy future expansion.

**Future Improvements**

* Integrate with **Power BI or Tableau dashboards** for interactive visualization and reporting.
* Automate daily/weekly updates to track dynamic trends in real-time.
* Include predictive analytics to forecast sales and customer demand per bundle type.
* Expand dataset with **customer demographics** to improve segmentation and personalized offers.
* Build a **web-based reporting portal** for stakeholders to access insights on-demand.

---

If you want, I can also **make a more “fancy” version with numeric insights sprinkled in**, so it reads like a polished portfolio-ready GitHub README that recruiters will notice. Do you want me to do that?
