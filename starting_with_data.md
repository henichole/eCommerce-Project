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
       
        ALTER TABLE all_sessions
        ADD COLUMN visitNumber INT;

        UPDATE all_sessions 
--I've been having issues with alias since day one, but that's okay, I'll just delete the 'as' alias for all_sessions.

        SET visitNumber = a.visitNumber
        FROM analytics a
        WHERE all_sessions.fullVisitorId = a.fullVisitorId;

--After adding the column "visitnumber" in, I realized there is another problem, since there are only 534 rows in the all_sessions table, and 34433 rows in the analytics, the PK and FK is different, so I cannot do that. The best result would be my rounded fullvisitorID in the analytics table. I manually looked for 1957460000000000000 and this fullbisitorID doesn't exist in the all_sessions table. There must be some data lost mistake during my cleaning process that I didn't know of.

Question 2: From the sales_report table, find the products skus that has high sold rate, low stock level and a long resctokingleadtime. 

SQL Queries:

        SELECT productSKU
        FROM sales_report
        WHERE total_ordered > 10 
        AND stocklevel > 0
        AND restockingLeadTime > 1;

Answer:
I found 75 productsskus, which can be analyzed by the stake holders to decide whether or not they should restock.


Question 3: Find the total number of unique visitors by referring sites, find each unique product viewed by each visitor, and compute the percentage of visitors to the site that actually makes a purchase

SQL Queries:

        SELECT
            channelgrouping,
            COUNT(DISTINCT fullVisitorId) AS unique_visitors,
            COUNT(DISTINCT CASE WHEN channelgrouping = 'Referral' THEN fullVisitorId END) AS referral_count
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

other 2 questions: pending


Question 4: In all_session table, change the column with values: "not available in demo dataset" to "null". And try to fix some of the duplicated countries & cities with same IDs (could be any IDs).


SQL Queries:

        UPDATE all_sessions
        SET city = NULL
        WHERE city = 'not available in demo dataset';

Answer:



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
"Tri-blend Hoodie Grey"
"Infant Short Sleeve Tee Royal Blue"
" Men's Short Sleeve Hero Tee Charcoal"
