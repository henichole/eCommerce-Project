Question 1: 
What is fullVisitorID with the highest visitNumber:

SQL Queries:

        SELECT
            fullVisitorID,
            MAX(visitNumber) AS highest_visitNumber
        FROM
            analytics
        GROUP BY
            fullVisitorID
        ORDER BY
            highest_visitNumber DESC
        LIMIT 1;

Answer: 
The visitor with an ID of "1957460000000000000" faced an issue because I use a Mac, and the analytics.csv file is too large for the Numbers application to open. When I opened it in Excel, the program automatically rounded this number to 6 significant figures. To address this problem, I attempted to resolve it by joining the "fullvisitorid" from the "all_sessions" table with the "fullvisitorid" in the "analytics" table.

        SELECT
            as.fullvisitoridnew AS fullvisitorid,
            MAX(a.visitnumber) AS highest_visitnumber   
        FROM
            all_sessions as
        JOIN
            analytics a ON as.fullVisitoridnew = a.fullVisitorid
        GROUP BY
            as.fullvisitoridnew
        ORDER BY
            highest_visitnumber DESC
        LIMIT 1;

--Since the previous solution didn't work, I added another column to the "all_sessions" table to import the "visitnumber" from the "analytics" table.
       
        ALTER TABLE 
                all_sessions
        ADD COLUMN 
                visitNumber INT;

        UPDATE 
                all_sessions 
--I've been encountering issues with aliases since day one, but that's alright. I'll simply remove the 'AS' alias for the "all_sessions" table.

        SET 
                visitNumber = a.visitNumber
        FROM 
                analytics a
        WHERE 
                all_sessions.fullVisitorId = a.fullVisitorId;

--After adding the "visitnumber" column, I've come to realize there's another issue. With only 534 rows in the "all_sessions" table and 34,433 rows in the "analytics" table, the primary key (PK) and foreign key (FK) don't match, making it impossible to join the tables this way. The most accurate approach would involve using the rounded "fullvisitorID" from the "analytics" table. Upon manual inspection, I found that 1957460000000000000 doesn't exist in the "all_sessions" table, suggesting there might have been some data loss or an oversight during my cleaning process that I wasn't aware of.

Question 2: From the sales_report table, find the products skus that has high sold rate, low stock level and a long restockingleadtime. 

SQL Queries:

        SELECT 
                productSKU
        FROM 
                sales_report
        WHERE 
                total_ordered > 10 
        AND 
                stocklevel > 0
        AND 
                restockingLeadTime > 1;
                
-I further refined the sorting criteria to be in ascending (ASC) order of stock level and descending (DESC) order of restocking lead time.

        SELECT 
                productSKU, stocklevel, restockingleadtime
        FROM 
                sales_report
        WHERE 
                total_ordered > 10 
        AND 
                stocklevel > 0
        AND 
                restockingLeadTime > 1
        ORDER BY 
        	stocklevel asc,
        	restockingleadtime
        LIMIT 8;

Answer:
I identified 8 products that meet specific criteria: they have a total order quantity exceeding 10, a stock level below 100, and a relatively extended restocking lead time. This information can be analyzed by the stakeholders to determine whether they should restock these items. The products meeting these conditions were retrieved using the following query:

        SELECT 
                name, 
        FROM 
                sales_report
        WHERE 
                productSKU IN ('GGOEGAAX0106', 'GGOEYAAJ033015', 'GGOEGAEJ031315', 'GGOEYAAJ033014',                             'GGOEGAAX0330', 'GGOEGAAL010616', 'GGOEGAAX0620', 'GGOEGAAL010614');

Answers:
The items that meet the criteria are "Men's 100% Cotton Short Sleeve Hero Tee Navy," "Men's Long & Lean Tee Charcoal," "Infant Short Sleeve Tee Green," and "Tri-blend Hoodie Grey."

Notice that there are duplications in the product names, which might be attributed to the "productvariant" (sizing) column in the "all_sessions" table. However, upon inspection, I found that most of the values in the "productvariant" column were null. Therefore, I decided to leave it as it is since I don't want to join potential products with different sizing, which is a reasonable approach.



Question 3: Find the total number of unique visitors by referring sites. Find each unique product viewed by each visitor, and compute the percentage of visitors to the site that actually makes a purchase

SQL Queries:

        SELECT
            channelgrouping,
            COUNT(DISTINCT fullVisitorId) AS unique_visitors,
            COUNT(DISTINCT CASE WHEN channelgrouping = 'Referral' THEN     fullVisitorId END) AS referral_count
        FROM
            analytics
        GROUP BY
            channelgrouping;
        	
Answer:
The total number of unique visitors by referring sites is 5928.
Other types are:
Organic Search: 18254
Direct: 5951
Social: 2551
Paid Search：1106
Affiliates：400
Display：242
Other：1 

For the second question, I used array to find each unique ID with the product viewed:

        WITH unique_products_per_visitor AS (
            SELECT
                fullVisitorId,
                productSKU,
                COUNT(DISTINCT productSKU) AS unique_product_count
            FROM
                all_sessions
            GROUP BY
                fullVisitorId, productSKU
        )
        SELECT
            fullVisitorId,
            ARRAY_AGG(DISTINCT productSKU) AS viewed_products
        FROM
            unique_products_per_visitor
        GROUP BY
            fullVisitorId;

Answer:
The query returned with 534 rows with fullvisitorId and viewed_products. For the last part, I do not think the percentage is worth finding because due to the data quality, most of the totaltransactionrevenue are null. I used this query to find the fullvisitorId with non null values of totaltransactionrevenue:

        SELECT 
                DISTINCT fullvisitorid
        FROM 
                all_sessions
        WHERE 
                totaltransactionrevenue is not null
Answer:
The query produced 534 rows with "fullvisitorId" and "viewed_products." However, for the final part of the analysis, you decided not to calculate the percentage because of the data quality issue. Given that most of the "totaltransactionrevenue" values are null, you used the following query to identify "fullvisitorId" with non-null values for "totaltransactionrevenue":

Question 4: In all_session table, change the column with values: "not available in demo dataset" to "null". And try to fix some of the duplicated countries & cities with same IDs (could be any IDs).

SQL Queries:

        UPDATE all_sessions
        SET city = NULL
        WHERE city = 'not available in demo dataset';

Answer:
The query did work and I changed every "not available in demo dataset" into "null".


Question 5: Find the productsku and product name with the best sentiment score and sentiment magnitude. And analyze if there is anything worth noting.

SQL Queries:

        WITH ranked_products AS (
    SELECT
        productSKU,
        name,
        sentimentScore,
        sentimentMagnitude,
        RANK() OVER (ORDER BY sentimentScore DESC, sentimentMagnitude DESC) AS score_rank
    FROM
        sales_report
        )
        SELECT
            productSKU,
            name,
            sentimentScore,
            sentimentMagnitude
        FROM
            ranked_products
        WHERE
            score_rank <= 3;

Answer: 
I identified 15 products with identical sentiment scores of 0.9 and sentiment magnitudes of 1.4. Here are a couple of the products from the list:

"Recycled Paper Journal Set"
"Women's Long Sleeve Blended Cardigan Charcoal"
Considering that both the sentiment score and sentiment magnitude are at 1.4, it seems there might not be a clear correlation or trend in the query. These values of 0.9 and 1.4 appear to be in the range of average performance according to open source results. Here are a few more products from the list:

"Tri-blend Hoodie Grey"
"Infant Short Sleeve Tee Royal Blue"
"Men's Short Sleeve Hero Tee Charcoal"
