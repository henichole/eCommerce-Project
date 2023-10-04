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


Answer:
Since the transactionrevenue has a 99.9% rate of nullality, I checked through the Excel to see what happened. 
after I cleaned the data, I think the column "transactionrevenue" is not that reliable or relevent. I could 
use the totaltransactionrevenue instead (I will show in the starting_with_data.md file).



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
Within the 27 rows of non-null values, average number of products ordered from visitors in each city and country ranged from 1 to 11. The top countries with most visitor orders are the cities from United States and Madrid, Spain(10). Majority of the orders are from cities of the US.
which add up to around 30 among the cities.


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
1."Home/Shop by Brand/YouTube/"                                -613 product_category_count
2."Home/Apparel/Men's/Men's-T-Shirts/"                         -435 product_category_count
3."Home/Apparel/"                                              -288 product_category_count
4."Home/Electronics/"                                          -280 product_category_count
5."Home/Shop by Brand/Google/"                                 -223 product_category_count

Majorities of the visitors are from the US, and according to the cities, since about half of the cities are null, and there are so many 
product categories, only the major cities in the US standed out with the Youtube purchases. This is an indication of eCommerce gradually shifting from retail to YouTube & Google searches. Other than the basic necessities such as appearels and electronics, influencers from
YouTube now play a huge role in marketing.


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
"Ballpoint LED Light Pen"                                       456*22 units sold in cities among the countries
"17oz Stainless Steel Sport Bottle"                             334*15 units sold in cities among the countries
"Leatherette Journal"                                           319*12 units sold in cities among the countries
"Foam Can and Bottle Cooler"                                   290*11 units sold in cities among the countries 


**Question 5: Can we summarize the impact of revenue generated from each city/country?**

SQL Queries attempt 1 via SUM, AVG, MAX, MIN:
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

SQL Queries attempt 2 via SUM:
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
"San Francisco"
"Sunnyvale"
"Atlanta"
These three cities ranged from 800 to 1500.
One Non-US city that standed out for me is "Tel Aviv-Yafo" from Israel, since I have all the SUM, AVG, MAX and MIN total_transaction_revenue to look at, this city only has one single transaction of 600. 
Despite of the usual big percentage of nullality, and abundancies in the US cities (not specified), I noticed that some cities like New York is labelled as "Canada", also, some districts like Shinjuku and Shibuya (in Tokyo) are labelled in the cities column, so I had to fix it.

Top Countries:
"US"
"Israel"
"Australia"
"Canada"
I didn't count Canada as the third place since "New York" was counted toward Canada, which was incorrect.


Overall, the impact of the transaction revenues is the most on the west coast, US, especially cities around the sillicon valley area.








