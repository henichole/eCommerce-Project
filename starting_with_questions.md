Answer the following questions and provide the SQL queries used to find the answer.

    
**Question 1: Which cities and countries have the highest level of transaction revenues on the site?**


SQL Queries:
SELECT
    country,
    city,
    SUM(transactionRevenue) AS total_transaction_revenue
FROM
    all_sessions
GROUP BY
    country,
    city
ORDER BY
    total_transaction_revenue DESC;



Answer:




**Question 2: What is the average number of products ordered from visitors in each city and country?**


SQL Queries:
SELECT
    country,
    city,
    AVG(productQuantity) AS avg_products_ordered
FROM
    all_sessions
GROUP BY
    country,
    city;



Answer:





**Question 3: Is there any pattern in the types (product categories) of products ordered from visitors in each city and country?**


SQL Queries:
SELECT
    country,
    city,
    v2ProductCategory,
    COUNT(*) AS product_category_count
FROM
    all_sessions
GROUP BY
    country,
    city,
    v2ProductCategory
ORDER BY
    country,
    city,
    product_category_count DESC;



Answer:





**Question 4: What is the top-selling product from each city/country? Can we find any pattern worthy of noting in the products sold?**


SQL Queries:
WITH ranked_products AS (
    SELECT
        a.country,
        a.city,
        p.name AS top_selling_product,
        p.SKU,
        s.total_ordered,
        ROW_NUMBER() OVER (PARTITION BY a.country, a.city ORDER BY s.total_ordered DESC) AS sales_rank
    FROM
        all_sessions a
    JOIN
        sales_by_sku s ON a.productSKU = s.productSKU
    JOIN
        products p ON a.productSKU = p.SKU
)
SELECT
    country,
    city,
    top_selling_product,
    SKU,
    total_ordered AS units_ordered
FROM
    ranked_products
WHERE
    sales_rank = 1;



Answer:





**Question 5: Can we summarize the impact of revenue generated from each city/country?**

SQL Queries:
SELECT
    country,
    city,
    SUM(transactionRevenue) AS total_transaction_revenue,
    AVG(transactionRevenue) AS avg_transaction_revenue,
    MAX(transactionRevenue) AS max_transaction_revenue,
    MIN(transactionRevenue) AS min_transaction_revenue
FROM
    all_sessions
GROUP BY
    country,
    city
ORDER BY
    country,
    city;



Answer:







