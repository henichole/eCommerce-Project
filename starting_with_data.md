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
The visitor who has an Id of 1957460000000000000, unfortunately, I use a mac, and the analytics.csv file is too big for Numbers application on Mac to open, when I open in Excel, Excel automatically rounded this number to 6 sig figs. I tried to solve this problem by joing all_sessions.fullvisitorid = analytics.fullvisitorid:

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

Question 2: From the sales_report table, find the products skus that has high sold rate, low stock level and a long resctokingleadtime. 

SQL Queries:

        SELECT productSKU
        FROM sales_report
        WHERE total_ordered > 10 
        AND stocklevel > 0
        AND restockingLeadTime > 1;

Answer:
I found 75 productsskus, which can be analyzed by the stake holders to decide whether or not they should restock.


Question 3: 

SQL Queries:

Answer:



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
