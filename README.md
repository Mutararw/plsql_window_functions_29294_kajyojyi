# -plsql_window_functions_-29294-_-kajyojyi-


1. Business Problem
Business Context

This project is based on a regional retail business that sells products to customers located in different regions. The business records customer details, product information, and daily sales transactions.

Data Challenge

Although sales transactions are recorded, management lacks analytical insights into customer purchasing behavior, product performance, and sales trends over time. The available data is not yet structured for decision-making.

Expected Outcome

Use SQL JOINs and Window Functions to analyze sales data, identify top-performing products and customers, and support business decisions related to marketing and inventory management.

2. Success Criteria

The analysis aims to achieve the following objectives:

Identify top-selling products using ranking window functions

Calculate cumulative sales over time

Analyze sales changes between consecutive periods

Segment customers into revenue-based groups

Evaluate average sales trends

3. Database Schema Design
Tables Overview
Customers Table

customer_id (Primary Key)

name

region

Products Table

product_id (Primary Key)

name

category

price

Sales Table

sales_id (Primary Key)

customer_id (Foreign Key)

product_id (Foreign Key)

sales_date

quantity

amount

Entity Relationship Diagram (ERD)

📌 ER Diagram

[ ADD ER DIAGRAM IMAGE HERE ]

4. Part A — SQL JOINs Implementation
4.1 INNER JOIN

Purpose: Retrieve sales records with valid customers and products.

SELECT c.name AS customer_name,
       p.name AS product_name,
       s.quantity,
       s.amount
FROM sales s
INNER JOIN customers c ON s.customer_id = c.customer_id
INNER JOIN products p ON s.product_id = p.product_id;


📸 Screenshot

[ ADD INNER JOIN RESULT SCREENSHOT HERE ]


🧠 Business Interpretation
This query ensures that only complete and valid sales transactions are analyzed by linking customers, products, and sales records.

4.2 LEFT JOIN

Purpose: Identify customers who have never made a purchase.

SELECT c.customer_id, c.name
FROM customers c
LEFT JOIN sales s ON c.customer_id = s.customer_id
WHERE s.sales_id IS NULL;


📸 Screenshot

[ ADD LEFT JOIN RESULT SCREENSHOT HERE ]


🧠 Business Interpretation
This query helps identify inactive customers who may require promotional or engagement strategies.

4.3 RIGHT JOIN

Purpose: Identify products that have never been sold.

SELECT p.product_id, p.name
FROM sales s
RIGHT JOIN products p ON s.product_id = p.product_id
WHERE s.sales_id IS NULL;


📸 Screenshot

[ ADD RIGHT JOIN RESULT SCREENSHOT HERE ]


🧠 Business Interpretation
Products with no sales may indicate low demand or pricing issues.

4.4 FULL OUTER JOIN

Purpose: Show all customers and sales, including unmatched records.

SELECT c.name, s.sales_id
FROM customers c
FULL OUTER JOIN sales s
ON c.customer_id = s.customer_id;


📸 Screenshot

[ ADD FULL JOIN RESULT SCREENSHOT HERE ]


🧠 Business Interpretation
This query provides a complete overview of customer participation and missing sales records.

4.5 SELF JOIN

Purpose: Compare customers within the same region.

SELECT a.name AS customer_1,
       b.name AS customer_2,
       a.region
FROM customers a
JOIN customers b
ON a.region = b.region
AND a.customer_id <> b.customer_id;


📸 Screenshot

[ ADD SELF JOIN RESULT SCREENSHOT HERE ]


🧠 Business Interpretation
Allows comparison of customers operating in the same region.

5. Part B — SQL Window Functions
5.1 Ranking Function

Top products by total sales amount

SELECT p.name,
       SUM(s.amount) AS total_sales,
       RANK() OVER (ORDER BY SUM(s.amount) DESC) AS sales_rank
FROM sales s
JOIN products p ON s.product_id = p.product_id
GROUP BY p.name;


📸 Screenshot

[ ADD RANK FUNCTION SCREENSHOT HERE ]


🧠 Interpretation
Ranks products based on their contribution to total revenue.

5.2 Aggregate Window Function

Running total of sales

SELECT sales_date,
       amount,
       SUM(amount) OVER (
           ORDER BY sales_date
           ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW
       ) AS running_total
FROM sales;


📸 Screenshot

[ ADD RUNNING TOTAL SCREENSHOT HERE ]


🧠 Interpretation
Displays how total sales accumulate over time.

5.3 Navigation Function

Compare current sales with previous sales

SELECT sales_date,
       amount,
       amount - LAG(amount) OVER (ORDER BY sales_date) AS sales_difference
FROM sales;


📸 Screenshot

[ ADD LAG FUNCTION SCREENSHOT HERE ]


🧠 Interpretation
Highlights increases or decreases between consecutive sales periods.

5.4 Distribution Function

Customer revenue segmentation

SELECT customer_id,
       NTILE(4) OVER (ORDER BY SUM(amount) DESC) AS revenue_quartile
FROM sales
GROUP BY customer_id;


📸 Screenshot

[ ADD NTILE FUNCTION SCREENSHOT HERE ]


🧠 Interpretation
Segments customers into four groups based on total revenue.

6. Results Analysis
Descriptive Analysis

The analysis shows variations in sales performance across products and customers.

Diagnostic Analysis

Higher revenue is driven by a small number of customers and products.

Prescriptive Analysis

The business should focus on retaining high-value customers and improving low-performing products.

7. References

Oracle SQL Documentation

PostgreSQL Window Functions Documentation

Database Systems Course Notes

8. Integrity Statement

“All sources were properly cited. The analysis and SQL implementations represent original academic work.”
