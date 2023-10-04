Answer the following questions and provide the SQL queries used to find the answer.

    
**Question 1: Which cities and countries have the highest level of transaction revenues on the site?**


SQL Queries:

            SELECT
                country,
                city,
              	SUM(CAST(transactionrevenue AS integer))
            FROM
                all_sessions
            where transactionrevenue IS NOT null
            GROUP BY
                country,
                city
            ORDER BY 3
            --indicates the number of column of selected table
                DESC;
                
--Only one country returned, I switched the column to totaltransactionrevenue and got 

Answer:
Since the transactionrevenue has a high nullality, I checked through the Excel to see what happened: I realized that the reason behind it was because a lot of orderids were created without having a transaction revenue. Therefore, I think the column "transactionrevenue" is not that reliable or relevent. I
use the totaltransactionrevenue instead and got a list of 6 cities from US and Canada, the top one is US, since a lot of the cities were missing, so it is a total of all null cities in the US.

Results:
country              city              totaltransactionrevenue
"United States"		                    112.6650000
"United States"	  "Chicago"	            123.940000
"Canada"	      "Toronto"	             82.160000
"United States"	  "New York"	         51.330000
"United States"	  "Mountain View"    	 43.810000
"United States"	  "Austin"	             35.780000
"United States"	  "Columbus"	         21.990000



**Question 2: What is the average number of products ordered from visitors in each city and country?**


SQL Queries:

            SELECT 
              country,
              city,
              AVG(CAST(productQuantity AS numeric)) AS avg_products_ordered
            FROM 
              all_sessions
            GROUP BY 
              country, 
              city
            HAVING AVG(CAST(productQuantity AS numeric)) IS NOT NULL;


Answer:
The average number of products ordered from visitors in each city and country are 1. But I don't feel right about this result, I tried to union the rows with same country and city name, along with the product quantity. 

            WITH deduplicated_all_sessions AS (
                SELECT DISTINCT country, city, productQuantity
                FROM all_sessions
            )
            SELECT 
                country,
                city,
                AVG(CAST(productQuantity AS numeric)) AS avg_products_ordered
            FROM 
                deduplicated_all_sessions
            GROUP BY 
                country, 
                city
            HAVING 
                AVG(CAST(productQuantity AS numeric)) IS NOT NULL;
            
--However, since most of the values under prduct quantity, I need to use total_ordered from sales_report.

country          city         
"United States"	"New York"	1.00000000000000000000
"India"	"Bengaluru"	1.00000000000000000000
"United States"		1.00000000000000000000
"United States"	"Detroit"	1.00000000000000000000
"United States"	"Columbus"	1.00000000000000000000
"Mexico"		1.00000000000000000000
"United States"	"Mountain View"	1.00000000000000000000
"United States"	"Chicago"	1.00000000000000000000


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
--I noted any of these three conditions to filter out the product_category_count.

Answer:
The top five product categories are:
1. Not set 
2. "Home/Apparel/Women's/Women's-T-Shirts/"
3. "Home/Apparel/"

Trends I see: For major cities in US and Canada, Home/Apparel is the most popular product category.


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
The top five star product are:
"Ballpoint LED Light Pen"                                       456 units sold in cities among the countries
"17oz Stainless Steel Sport Bottle"                             334 units sold in cities among the countries
"Leatherette Journal"                                           319 units sold in cities among the countries
"Foam Can and Bottle Cooler"                                   290 units sold in cities among the countries 


**Question 5: Can we summarize the impact of revenue generated from each city/country?**

--SQL Queries attempt 1 via SUM, AVG, MAX, MIN:

            SELECT
                country,
                city,
                SUM(CAST(totalTransactionRevenue AS NUMERIC)) AS total_transaction_revenue,
                AVG(CAST(totalTransactionRevenue AS NUMERIC)) AS avg_transaction_revenue,
                MAX(CAST(totalTransactionRevenue AS NUMERIC)) AS max_transaction_revenue,
                MIN(CAST(totalTransactionRevenue AS NUMERIC)) AS min_transaction_revenue
            FROM
                all_sessions
            GROUP BY
                country,
                city
            ORDER BY
                country,
                city DESC;

--SQL Queries attempt 2 via SUM:

            SELECT
                country,
                city,
                SUM(CAST(totalTransactionRevenue AS NUMERIC)) AS total_revenue 
            FROM
                all_sessions
            GROUP BY
                country,
                city
            ORDER BY
            	country,
                total_revenue DESC;

    
Answer: 
Top Cities
"Chicago"
"Toronto"
"New York"

Top Country:
"US"










