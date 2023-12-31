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
                
--When I switched the column to "totaltransactionrevenue," I received the following result:

Answer: The high presence of null values in the "transactionrevenue" column prompted me to investigate further. Upon checking the data in Excel, I discovered that many order IDs were created without corresponding transaction revenues. As a result, I concluded that the "transactionrevenue" column might not be a reliable or relevant metric for analysis. I opted to use "totaltransactionrevenue" instead and obtained a list of 6 cities from the United States and Canada. The top-ranking city is the United States. Since many cities were missing, the result represents a sum of all null cities in the United States.

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

--The average number of products ordered from visitors in each city and country appears to be 1, but I have reservations about this result. To address my concerns, I attempted to combine the rows with the same country and city name, along with the product quantity.

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
            
--As expected, the average order number was consistently 1 for every country, given the many null values under the "product quantity." To obtain a more accurate average order number, I'll need to utilize the "total_ordered" from the "sales_report" by using a JOIN operation.

        WITH deduplicated_all_sessions AS (
            SELECT DISTINCT country, city, productSKU
            FROM all_sessions
        )
        SELECT 
            a.country,
            a.city,
            AVG(sr.total_ordered) AS avg_products_ordered
        FROM 
            deduplicated_all_sessions a
        JOIN
            sales_report sr ON a.productSKU = sr.productSKU
        GROUP BY 
            a.country, 
            a.city
        HAVING 
            AVG(sr.total_ordered) IS NOT NULL;
--Here I got 83 rows, I then refined it down to top 5 US cities:

        WITH deduplicated_all_sessions AS (
            SELECT DISTINCT country, city, productSKU
            FROM all_sessions
        )
        SELECT 
            a.country,
            a.city,
            AVG(sr.total_ordered) AS avg_products_ordered
        FROM 
            deduplicated_all_sessions a
        JOIN
            sales_report sr ON a.productSKU = sr.productSKU
        GROUP BY 
            a.country, 
            a.city
        HAVING 
            AVG(sr.total_ordered) IS NOT NULL
        ORDER BY
            CASE WHEN a.country = 'United States' THEN 1 ELSE 2 END, -- Sort US cities first
            avg_products_ordered DESC 
        -- Sort by average products ordered in descending order
        LIMIT 5

Answer:
--TOP 5 cities with highest average order numbers, there were all US cities.

"Austin"	169.5000000000000000
"Kalamazoo"	105.0000000000000000
"San Antonio"	85.0000000000000000
"Detroit"	23.0000000000000000
"Pittsburgh"	19.5000000000000000

--I spiced up a little bit with non US cities with a WHERE clause:

        WITH deduplicated_all_sessions AS (
            SELECT DISTINCT country, city, productSKU
            FROM all_sessions
        )
        SELECT 
            a.country,
            a.city,
            AVG(sr.total_ordered) AS avg_products_ordered
        FROM 
            deduplicated_all_sessions a
        JOIN
            sales_report sr ON a.productSKU = sr.productSKU
        WHERE 
            a.country != 'United States' -- Exclude US cities
        GROUP BY 
            a.country, 
            a.city
        HAVING 
            AVG(sr.total_ordered) IS NOT NULL
        ORDER BY
            avg_products_ordered DESC
        LIMIT 5; -- Limit to top 5 non-US cities
        
Top 5 cities, non US with highest average order number:
"Kuwait", null,	334.0000000000000000
"Indonesia", null, 290.0000000000000000
"Germany", "Hamburg", 189.0000000000000000
"Japan", "Minato", 78.5000000000000000
"Germany", null, 56.8000000000000000



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

The trends I've observed are as follows: In major cities in the United States and Canada, the Home/Apparel product category appears to be the most popular. However, I'm uncertain about the accuracy of this finding. It's possible that I missed something during the data cleaning process. Alternatively, given the vast number of product categories, the data itself may be a contributing factor to the challenge in pinpointing the most popular category with certainty.


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

--SQL Queries attempt 2 via SUM, I also forget to divide by 1000000:

            SELECT
                country,
                city,
                SUM(CASE WHEN totalTransactionRevenue IS NOT NULL THEN CAST(totalTransactionRevenue AS NUMERIC)                     / 1000000 ELSE 0 END) AS total_revenue_in_new
            FROM
                all_sessions
            WHERE
                totalTransactionRevenue IS NOT NULL
            GROUP BY
                country,
                city
            ORDER BY
                country,
                total_revenue_in_new DESC;

    
Answer: 
Top Cities
"Chicago"
"Toronto"
"New York"

Top Country:
"US"
            










