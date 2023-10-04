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
The visitor who has an Id of "1957460000000000000", unfortunately, I use a mac, and the analytics.csv file is too big for Numbers application on Mac to open, when I open in Excel, Excel automatically rounded this number to 6 sig figs. I tried to solve this problem by joing all_sessions.fullvisitorid = analytics.fullvisitorid:

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

--Since above didn't work, I added another column in all_sesions table to port in the "visitnumber" from analytics:
       
        ALTER TABLE 
                all_sessions
        ADD COLUMN 
                visitNumber INT;

        UPDATE 
                all_sessions 
--I've been having issues with alias since day one, but that's okay, I'll just delete the 'as' alias for all_sessions.

        SET 
                visitNumber = a.visitNumber
        FROM 
                analytics a
        WHERE 
                all_sessions.fullVisitorId = a.fullVisitorId;

--After adding the column "visitnumber" in, I realized there is another problem, since there are only 534 rows in the all_sessions table, and 34433 rows in the analytics, the PK and FK is different, so I cannot do that. The best result would be my rounded fullvisitorID in the analytics table. I manually looked for 1957460000000000000 and this fullbisitorID doesn't exist in the all_sessions table. There must be some data lost mistake during my cleaning process that I didn't know of.

Question 2: From the sales_report table, find the products skus that has high sold rate, low stock level and a long resctokingleadtime. 

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
                
-I further narrowed it down to ASC order of stocklevel, and DESC order of restockingleadtime

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
I found the 8 products that has a total ordered number bigger than 10, a stocklevel less than 100, and a relatively longer restockingleadtime, which can be analyzed by the stake holders to decide whether or not they should restock. These items are then found using the query:

        SELECT 
                name, 
        FROM 
                sales_report
        WHERE 
                productSKU IN ('GGOEGAAX0106', 'GGOEYAAJ033015', 'GGOEGAEJ031315', 'GGOEYAAJ033014',                             'GGOEGAAX0330', 'GGOEGAAL010616', 'GGOEGAAX0620', 'GGOEGAAL010614');

Answers:
The items are " Men's 100% Cotton Short Sleeve Hero Tee Navy", " Men's Long & Lean Tee Charcoal", " Infant Short Sleeve Tee Green" and " Tri-blend Hoodie Grey". 

There are duplications in the names of the product, so I figured this might be due to productvariant (sizing) column in the all_sessions table. I checked the all_sessions table and found most of the values in the productvariant columns were also null. Therefore, I decided to leave it there since I don't want to join potential products in different sizing.



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
The query returend with only 11 rows, which matched with my hypothesis, there is no point of finding the percentages using this way. I would have to dig deeper into the visitstarttime,timeonsite and total_ordered columns to find out. This is not a good question for this particular data set.

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
I found 15 products with identical sentiment score of 0.9 and sentiment magnitude of 1.4:
Some of these are listed below:
"Recycled Paper Journal Set"
"Women's Long Sleeve Blended Cardigan Charcoal"

Since both sentiment score and sentiment magnitude is 1.4, I don't see a correlation nor trend in my question. 0.9 and 1.4 were at an average performance, according to open source results.
"Tri-blend Hoodie Grey"
"Infant Short Sleeve Tee Royal Blue"
" Men's Short Sleeve Hero Tee Charcoal"
