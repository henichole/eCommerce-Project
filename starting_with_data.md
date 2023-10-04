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


Question 3: Find the top products with good sentimental magnitude and sentimentscore, then relate the product to the product category.

SQL Queries:

Answer:



Question 4: In all_session table, change the column with values: "not available in demo dataset" to "null". And try to fix some of the duplicated countries & cities with same IDs (could be any IDs).


SQL Queries:

        UPDATE all_sessions
        SET city = NULL
        WHERE city = 'not available in demo dataset';

Answer:



Question 5: Find the productsku and product name with the best 3 sentiment score and sentiment magnitude.

SQL Queries:

Answer:
