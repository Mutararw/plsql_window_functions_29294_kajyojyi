# SQL JOINs & Window Functions Project

## 1. Business Problem

### Business Context
This project is based on a regional retail business that sells products to customers located in different regions. The business maintains data about customers, products, and sales transactions.

### Data Challenge
Although sales data is recorded daily, management lacks clear analytical insights into customer behavior, product performance, and sales trends over time.

### Expected Outcome
Use SQL JOINs and Window Functions to generate analytical reports that support better business and decision-making processes.

---

## 2. Success Criteria

1. Identify top-selling products using ranking window functions  
2. Calculate cumulative sales using aggregate window functions  
3. Analyze sales changes between periods using navigation functions  
4. Segment customers into revenue-based groups  
5. Analyze average sales trends over time  

---

## 3. Database Schema Design

The database for this project contains three related tables: **Customers**, **Products**, and **Sales**, designed to support sales analysis using SQL JOINs and window functions. The **Customers** table stores customer identity and region information, while the **Products** table stores product details such as category and price. The **Sales** table records transactions and connects customers and products using foreign keys. Primary keys uniquely identify records in each table, and foreign keys maintain relationships between tables. This relational structure allows efficient querying, reporting, and business analysis.


---

## 4. Entity Relationship Diagram (ERD)

![diagram](public/diagram.png)

---

## 5. Part A - SQL JOINs

### INNER JOIN
```sql
SELECT c.name, p.name, s.amount
FROM sales s
INNER JOIN customers c ON s.customer_id = c.customer_id
INNER JOIN products p ON s.product_id = p.product_id;
```

![INNER JOIN](public/inner_join.png)

---

### LEFT JOIN
```sql
SELECT c.customer_id, c.name
FROM customers c
LEFT JOIN sales s ON c.customer_id = s.customer_id
WHERE s.sales_id IS NULL;
```

![left JOIN](public/left_join.png)

---

### RIGHT JOIN
```sql
SELECT p.product_id, p.name
FROM sales s
RIGHT JOIN products p ON s.product_id = p.product_id
WHERE s.sales_id IS NULL;
```

![right JOIN](public/right_join.png)

---

### FULL OUTER JOIN
```sql
SELECT c.name, s.sales_id
FROM customers c
FULL OUTER JOIN sales s
ON c.customer_id = s.customer_id;
```

![full outer JOIN](public/full_outer_join.png)

---

### SELF JOIN
```sql
SELECT a.name, b.name, a.region
FROM customers a
JOIN customers b
ON a.region = b.region
AND a.customer_id <> b.customer_id;
```

![self JOIN](public/self_join.png)

---

## 6. Part B - Window Functions

### Ranking Function
```sql
SELECT p.name,
       SUM(s.amount) AS total_sales,
       RANK() OVER (ORDER BY SUM(s.amount) DESC) AS rank
FROM sales s
JOIN products p ON s.product_id = p.product_id
GROUP BY p.name;
```

![ranking function](public/rank_function.png)

---

### Aggregate Function
```sql
SELECT sales_date,
       amount,
       SUM(amount) OVER (ORDER BY sales_date) AS running_total
FROM sales;
```

![aggregate function](public/aggregate_function.png)

---

### Navigation Function
```sql
SELECT sales_date,
       amount,
       amount - LAG(amount) OVER (ORDER BY sales_date) AS difference
FROM sales;
```

![navigation function](public/navigation_function.png)

---

### Distribution Function
```sql
SELECT customer_id,
       NTILE(4) OVER (ORDER BY SUM(amount) DESC) AS quartile
FROM sales
GROUP BY customer_id;
```

![distribution function](public/distribution_function.png)

---

## Results Analysis

### Descriptive
Sales performance varies across products and customers, with a small number of products contributing to most of the total sales revenue.

### Diagnostic
Higher revenue concentration is mainly driven by repeat purchases from certain customers and consistent sales of specific product categories.

### Prescriptive
The business should focus on retaining high-value customers, promoting top-selling products, and improving marketing strategies for low-performing products.

---

## References
PostgreSQL Official Documentation  
SQL Window Functions Documentation  

---

## Integrity Statement
All sources were properly cited. Implementations and analysis represent original work. No AI-generated content was copied without attribution or adaptation.
