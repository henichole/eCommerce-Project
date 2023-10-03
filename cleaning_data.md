What issues will you address by cleaning the data?
--Join duplicate columns
1. Join productSKU and total_ordered from sales_report, sales_by_SKU, and all_sessions
   SELECT
    sr.productSKU,
    sr.total_ordered AS total_ordered_sales_report,
    sbs.total_ordered AS total_ordered_sales_by_sku,
    asession.total_ordered AS total_ordered_all_sessions
FROM
    sales_report sr
JOIN
    sales_by_sku sbs ON sr.productSKU = sbs.productSKU
JOIN
    all_sessions asession ON sr.productSKU = asession.productSKU;

2.Joining name from sales_report, all_sessions, and products:
SELECT
    sr.name AS name_sales_report,
    asession.name AS name_all_sessions,
    p.name AS name_products
FROM
    sales_report sr
JOIN
    all_sessions asession ON sr.productSKU = asession.productSKU
JOIN
    products p ON sr.productSKU = p.SKU;

3.Joining stockLevel and restockingLeadTime from sales_report and products:
SELECT
    sr.stockLevel AS stock_level_sales_report,
    sr.restockingLeadTime AS restocking_lead_time_sales_report,
    p.stockLevel AS stock_level_products,
    p.restockingLeadTime AS restocking_lead_time_products
FROM
    sales_report sr
JOIN
    products p ON sr.productSKU = p.SKU;

4.Joining sentimentScore, sentimentMagnitude, and ratio from sales_report and products:
SELECT
    sr.sentimentScore AS sentiment_score_sales_report,
    sr.sentimentMagnitude AS sentiment_magnitude_sales_report,
    sr.ratio AS ratio_sales_report,
    p.sentimentScore AS sentiment_score_products,
    p.sentimentMagnitude AS sentiment_magnitude_products,
    p.ratio AS ratio_products
FROM
    sales_report sr
JOIN
    products p ON sr.productSKU = p.SKU;

5.Joining sentimentScore, sentimentMagnitude, and ratio from sales_report and products:
SELECT
    sr.sentimentScore AS sentiment_score_sales_report,
    sr.sentimentMagnitude AS sentiment_magnitude_sales_report,
    sr.ratio AS ratio_sales_report,
    p.sentimentScore AS sentiment_score_products,
    p.sentimentMagnitude AS sentiment_magnitude_products,
    p.ratio AS ratio_products
FROM
    sales_report sr
JOIN
    products p ON sr.productSKU = p.SKU;

6.Rank the total revenue, orders placed in descending order
for the total revenue:
SELECT
    country,
    city,
    totalTransactionRevenue AS revenue,
    ROW_NUMBER() OVER (ORDER BY totalTransactionRevenue DESC) AS revenue_rank
FROM
    all_sessions;

for the order placed:
SELECT
    country,
    city,
    transactions AS orders_placed,
    ROW_NUMBER() OVER (ORDER BY transactions DESC) AS orders_rank
FROM
    all_sessions;
    
7. Clear null values
Replacing null values with set values:
SELECT
    COALESCE(column_name, replacement_value) AS cleaned_column
FROM
    your_table;

flter out rows with null values using WHERE clause:
SELECT
    COALESCE(column_name, replacement_value) AS cleaned_column
FROM
    your_table;

9. Change the unit price by /1000000

UPDATE all_sessions
SET product_price = all_sessions.productPrice / 1000000;

10. What on earth is going on with all kinds of IDs in different tables?
   Columns I see in the all_sessions table:
      fullvisitorID (17-18 digits ID number)
   COlumns I see in the analytics table:
      visitNumber (SMALLINT of 2 digits)
      visitID(10 digits ID number)

I joined values in different columns from different tables that are the same.



Queries:
Below, provide the SQL queries you used to clean your data.


