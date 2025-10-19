

# TELCO OFFER, LOAD & TRANSACTION DATA ANALYSIS

**BY: MAYNUL HASSAN NISSAN**

---

## 1. Which Product is Most Ordered Per Bundle Type?

```sql
WITH BOX AS (
    SELECT
        BUNDLE_TYPE,
        CONCAT(UPPER(OPERATOR), "-", PRODUCT_NAME) AS PRODUCT_NAME,
        COUNT(ORDER_ID) AS TOTAL_ORDER,
        ROW_NUMBER() OVER (PARTITION BY BUNDLE_TYPE ORDER BY COUNT(ORDER_ID) DESC) AS RANKS
    FROM
        organic-reef-427307-i3.offer_load.Offer_load_Transaction
    GROUP BY 1,2
)
SELECT * EXCEPT(RANKS)
FROM BOX 
WHERE RANKS=1;
```

---

## 2. Which Bundle Type is Least Ordered in Each Division?

```sql
WITH Y AS (
    SELECT
        BUNDLE_TYPE,
        DIVISION,
        CONCAT(UPPER(OPERATOR), "- ", PRODUCT_NAME) AS PRODUCT_NAME,
        COUNT(ORDER_ID) AS TOTAL_ORDER,
        ROW_NUMBER() OVER (PARTITION BY DIVISION ORDER BY COUNT(ORDER_ID) DESC) AS RANKS
    FROM
        organic-reef-427307-i3.offer_load.Offer_load_Transaction
    GROUP BY 1,2,3
)
SELECT * EXCEPT(RANKS)
FROM Y
WHERE RANKS = 1;
```

---

## 3. How Many Products are There per Bundle Type?

```sql
SELECT
    BUNDLE_TYPE,
    COUNT(DISTINCT PRODUCT_ID) AS TOTAL_PRODUCTS
FROM
    organic-reef-427307-i3.offer_load.Offer_load_Transaction
GROUP BY 1;
```

---

## 4. Total Revenue and Average Selling Price per Product, Bundle Type, Operator, and Division

```sql
SELECT
    DIVISION,
    OPERATOR,
    SUM(CASE WHEN BUNDLE_TYPE = 'Internet' THEN SELLING_PRICE ELSE 0 END) AS TOTAL_REVENUE_INTERNET,
    AVG(CASE WHEN BUNDLE_TYPE = 'Internet' THEN SELLING_PRICE ELSE 0 END) AS AVG_SELLING_PRICE_INTERNET,
    SUM(CASE WHEN BUNDLE_TYPE = 'Intenet & Minute' THEN SELLING_PRICE ELSE 0 END) AS TOTAL_REVENUE_INTERNET_AND_MINUTE,
    AVG(CASE WHEN BUNDLE_TYPE = 'Intenet & Minute' THEN SELLING_PRICE ELSE 0 END) AS AVG_SELLING_PRICE_INTERNET_AND_MINUTE,
    SUM(CASE WHEN BUNDLE_TYPE = 'Minute' THEN SELLING_PRICE ELSE 0 END) AS TOTAL_REVENUE_MINUTE,
    AVG(CASE WHEN BUNDLE_TYPE = 'Minute' THEN SELLING_PRICE ELSE 0 END) AS AVG_SELLING_PRICE_MINUTE
FROM
    `organic-reef-427307-i3.offer_load.Offer_load_Transaction`
WHERE
    DIVISION IS NOT NULL
GROUP BY 1, 2;
```

---

## 5. Product with Highest Number of Sales & Total Revenue

```sql
SELECT
    CONCAT(UPPER(OPERATOR), "-", PRODUCT_NAME) AS PRODUCT_NAME,
    COUNT(ORDER_ID) AS COUNTS,
    SUM(SELLING_PRICE) AS TOTAL_REVENUE
FROM
    `organic-reef-427307-i3.offer_load.Offer_load_Transaction`
GROUP BY 1
ORDER BY COUNTS DESC
LIMIT 1;
```

---

## 6. Product & Bundle Type Contributing Highest Percentage of Total Revenue

```sql
WITH X AS (
    SELECT
        BUNDLE_TYPE,
        CONCAT(UPPER(OPERATOR), "-", PRODUCT_NAME) AS PRODUCT_NAME,
        SUM(SELLING_PRICE) AS TOTAL_REVENUE,
        ROUND(SUM(SELLING_PRICE) / (SELECT SUM(SELLING_PRICE) FROM `organic-reef-427307-i3.offer_load.Offer_load_Transaction`) * 100, 2) AS PERCENTAGE,
        ROW_NUMBER() OVER (ORDER BY SUM(SELLING_PRICE) DESC) AS RANKS
    FROM
        `organic-reef-427307-i3.offer_load.Offer_load_Transaction`
    GROUP BY BUNDLE_TYPE, OPERATOR, PRODUCT_NAME
)
SELECT *
FROM X
WHERE RANKS = 1;
```

---

## 7. Maximum & Minimum Discount and Commission per Product

```sql
SELECT
    CONCAT(UPPER(OPERATOR), "-", PRODUCT_NAME) AS PRODUCT_NAME,
    MAX(commission) AS MAX_COMMISSION,
    MIN(commission) AS MIN_COMMISSION,
    MAX(discount) AS MAX_DISCOUNT,
    MIN(discount) AS MIN_DISCOUNT
FROM
    `organic-reef-427307-i3.offer_load.Offer_load_Transaction`
GROUP BY 1;
```

---

## 8. Operator & Division with Highest Avg Discount Across Products

```sql
SELECT
    UPPER(OPERATOR) AS OPERATOR,
    UPPER(DIVISION) AS DIVISION,
    ROUND(AVG(DISCOUNT),2) AS AVG_DISCOUNT
FROM
    `organic-reef-427307-i3.offer_load.Offer_load_Transaction`
GROUP BY 1,2
ORDER BY AVG_DISCOUNT DESC
LIMIT 1;
```

---

## 9. Unique Customers and Repeat Buyers

```sql
SELECT
    CUSTOMER_ID,
    COUNT(DISTINCT DATE(ORDER_TIME)) AS ORDER_COUNT
FROM
    organic-reef-427307-i3.offer_load.Offer_load_Transaction
GROUP BY 1
HAVING ORDER_COUNT > 1;
```

---

## 10. Customer with Highest Orders / Total Spending

```sql
SELECT
    CUSTOMER_ID,
    COUNT(ORDER_ID) AS TOTAL_ORDERS,
    SUM(SELLING_PRICE) AS TOTAL_SPENDING
FROM
    organic-reef-427307-i3.offer_load.Offer_load_Transaction
GROUP BY 1
ORDER BY TOTAL_ORDERS DESC
LIMIT 1;
```

---

## 11. Average Purchase Frequency of Customers by Bundle Type

```sql
WITH CTE AS (
    SELECT
        BUNDLE_TYPE,
        CUSTOMER_ID,
        COUNT(ORDER_ID) AS PURCHASE_FREQUENCY
    FROM
        organic-reef-427307-i3.offer_load.Offer_load_Transaction
    GROUP BY 1,2
)
SELECT
    BUNDLE_TYPE,
    CASE 
        WHEN CTE.PURCHASE_FREQUENCY BETWEEN 1 AND 5 THEN '1-5'
        WHEN CTE.PURCHASE_FREQUENCY BETWEEN 6 AND 10 THEN '6-10'
        WHEN CTE.PURCHASE_FREQUENCY BETWEEN 11 AND 15 THEN '11-15'
        WHEN CTE.PURCHASE_FREQUENCY BETWEEN 16 AND 20 THEN '16-20'
        WHEN CTE.PURCHASE_FREQUENCY BETWEEN 21 AND 25 THEN '21-25'
        WHEN CTE.PURCHASE_FREQUENCY BETWEEN 26 AND 30 THEN '26-30'
        WHEN CTE.PURCHASE_FREQUENCY BETWEEN 31 AND 35 THEN '31-35'
        WHEN CTE.PURCHASE_FREQUENCY BETWEEN 36 AND 40 THEN '36-40'
        WHEN CTE.PURCHASE_FREQUENCY BETWEEN 41 AND 45 THEN '41-45'
        WHEN CTE.PURCHASE_FREQUENCY BETWEEN 46 AND 50 THEN '46-50'
        ELSE 'ABOVE 50' 
    END AS FREQUENCY_BIN,
    COUNT(DISTINCT CUSTOMER_ID) AS COUNT,
    ROUND(AVG(PURCHASE_FREQUENCY),2) AS AVG_PURCHASE
FROM
    CTE
GROUP BY 1,2;
```

---

## 12. Most Orders by Hour of the Day

```sql
SELECT
    EXTRACT(HOUR FROM ORDER_TIME) AS ORDER_HOUR,
    COUNT(ORDER_ID) AS TOTAL_ORDERS
FROM
    organic-reef-427307-i3.offer_load.Offer_load_Transaction
GROUP BY 1
ORDER BY TOTAL_ORDERS DESC
LIMIT 1;
```

---

## 13. Average Fulfillment / Delivery Time per Operator and Division

```sql
SELECT
    UPPER(OPERATOR) AS OPERATOR,
    ROUND(AVG(DATE_DIFF(DELIVERY_TIME, ORDER_TIME, MINUTE)),2) AS DELIVERY_TIME
FROM
    organic-reef-427307-i3.offer_load.Offer_load_Transaction
WHERE
    ORDER_STATUS = "Confirm"
GROUP BY 1;
```

---

## 14. Average Delivery Time per Division

```sql
SELECT
    UPPER(DIVISION),
    ROUND(AVG(DATE_DIFF(DELIVERY_TIME, ORDER_TIME, MINUTE)),2) AS DELIVERY_TIME
FROM
    organic-reef-427307-i3.offer_load.Offer_load_Transaction
WHERE
    ORDER_STATUS = 'Confirm'
GROUP BY 1;
```

---

## 15. Cancellation Rate & Complaint Rate per Operator & Division

```sql
WITH X AS (
    SELECT
        DIVISION,
        OPERATOR,
        COUNT(ORDER_ID) AS TOTAL_ORDERS,
        COUNT(DISTINCT CASE WHEN ORDER_STATUS = "Cancel" THEN ORDER_ID ELSE NULL END) AS CANCELLED_ORDERS,
        COUNT(DISTINCT CASE WHEN ORDER_STATUS = "Complain" THEN ORDER_ID ELSE NULL END) AS COMPLAINT_ORDERS
    FROM
        organic-reef-427307-i3.offer_load.Offer_load_Transaction
    WHERE DIVISION IS NOT NULL
    GROUP BY 1,2
)
SELECT
    DIVISION,
    OPERATOR,
    ROUND(CANCELLED_ORDERS / X.TOTAL_ORDERS * 100,2) AS CANCEL_PERCENTAGE,
    ROUND(COMPLAINT_ORDERS / X.TOTAL_ORDERS * 100,2) AS COMPLAINT_PERCENTAGE
FROM X;
```

---

## 16. Most Common Complaint Reasons

```sql
SELECT
    REASON,
    COUNT(REASON) AS MOST_COMMON_REASON
FROM
    organic-reef-427307-i3.offer_load.Offer_load_Transaction
GROUP BY 1
ORDER BY MOST_COMMON_REASON DESC
LIMIT 1;
```

---

## 17. Total Orders per Division

```sql
SELECT
    DIVISION,
    COUNT(ORDER_ID) AS TOTAL_ORDERS
FROM
    organic-reef-427307-i3.offer_load.Offer_load_Transaction
WHERE DIVISION IS NOT NULL
GROUP BY 1;
```

---

## 18. Division with Highest Sales & Transactions

```sql
SELECT
    DIVISION,
    SUM(SELLING_PRICE) AS TOTAL_SALES_VALUE,
    COUNT(ORDER_ID) AS TOTAL_TRANSACTIONS
FROM
    organic-reef-427307-i3.offer_load.Offer_load_Transaction
GROUP BY DIVISION
ORDER BY TOTAL_SALES_VALUE DESC
LIMIT 1;
```

---

## 19. Order Status Percentage per Operator & Division

```sql
WITH Y AS (
    SELECT
        DIVISION,
        OPERATOR,
        COUNT(ORDER_ID) AS TOTAL_ORDERS,
        COUNT(DISTINCT CASE WHEN ORDER_STATUS = "Confirm" THEN ORDER_ID ELSE NULL END) AS TOTAL_CONFIRMED,
        COUNT(DISTINCT CASE WHEN ORDER_STATUS = "Cancel" THEN ORDER_ID ELSE NULL END) AS TOTAL_CANCELED
    FROM
        organic-reef-427307-i3.offer_load.Offer_load_Transaction
    WHERE DIVISION IS NOT NULL
    GROUP BY 1,2
)
SELECT
    UPPER(DIVISION) AS DIVISION,
    UPPER(OPERATOR) AS OPERATOR,
    ROUND(TOTAL_CONFIRMED / Y.TOTAL_ORDERS * 100,2) AS COMPLETION_PERCENTAGE,
    ROUND(TOTAL_CANCELED / Y.TOTAL_ORDERS * 100,2) AS CANCELLATION_PERCENTAGE
FROM Y;
```

---

## 20. Products Offered by Each Operator

```sql
SELECT
    UPPER(OPERATOR) AS OPERATOR,
    COUNT(DISTINCT PRODUCT_ID) AS TOTAL_PRODUCTS
FROM
    organic-reef-427307-i3.offer_load.Offer_load_Transaction
GROUP BY 1;
```

---

## 21. Product Popularity by Validity Period

```sql
SELECT
    VALIDITY,
    COUNT(ORDER_ID) AS POPULARITY
FROM
    organic-reef-427307-i3.offer_load.Offer_load_Transaction
GROUP BY 1
ORDER BY POPULARITY DESC;
```

---

## 22. Advanced Product Popularity using CTE & Window Function

```sql
WITH M AS (
    SELECT
        VALIDITY,
        CONCAT(UPPER(OPERATOR), "-", PRODUCT_NAME) AS PRODUCT_NAME,
        COUNT(ORDER_ID) AS POPULARITY,
        ROW_NUMBER() OVER (PARTITION BY VALIDITY ORDER BY COUNT(ORDER_ID) DESC) AS POPULARITY_BY_VALIDITY
    FROM
        `organic-reef-427307-i3.offer_load.Offer_load_Transaction`
    GROUP BY VALIDITY, OPERATOR, PRODUCT_NAME
)
SELECT
    VALIDITY,
    PRODUCT_NAME,
    POPULARITY,
    POPULARITY_BY_VALIDITY
FROM M
ORDER BY POPULARITY DESC;
```

---

## 23. Products with Most Complaints & Cancellations

```sql
SELECT
    CONCAT(UPPER(OPERATOR), "-", PRODUCT_NAME) AS PRODUCT_NAME,
    COUNT(COMPLAIN_STATUS) AS TOTAL_COMPLAINTS,
    COUNT(CASE WHEN ORDER_STATUS = "Cancel" THEN ORDER_ID ELSE NULL END) AS TOTAL_CANCELLATIONS
FROM
    organic-reef-427307-i3.offer_load.Offer_load_Transaction
GROUP BY 1
ORDER BY TOTAL_COMPLAINTS DESC, TOTAL_CANCELLATIONS DESC
LIMIT 1;
```

---

## 24. Potential Revenue Loss Due to Cancellation

```sql
SELECT
    SUM(SELLING_PRICE) AS TOTAL_REVENUE_LOSS
FROM
    organic-reef-427307-i3.offer_load.Offer_load_Transaction
WHERE ORDER_STATUS = "Cancel";
```

---

## 25. Top 3 or 5 Most-Selling Products/Bundles in Key Divisions

```sql
WITH Z AS (
    SELECT
        DIVISION,
        PRODUCT_NAME,
        BUNDLE_TYPE,
        COUNT(ORDER_ID) AS TOTAL_SELLING,
        ROW_NUMBER() OVER (PARTITION BY DIVISION ORDER BY COUNT(ORDER_ID) DESC) AS RANKS
    FROM
        organic-reef-427307-i3.offer_load.Offer_load_Transaction
    WHERE DIVISION IN ('Dhaka','Rangpur')
    GROUP BY 1,2,3
    ORDER BY DIVISION, TOTAL_SELLING DESC
)
SELECT
    DIVISION,
    PRODUCT_NAME,
    BUNDLE_TYPE,
    Z.TOTAL_SELLING
FROM Z
WHERE RANKS <= 5;
```

---


