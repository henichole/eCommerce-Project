What issues will you address by cleaning the data?
1. I recognized the duplications present in the tables, visitorId, fullVisitorId and ProductSKU. So I pulled the table out to identify them, then created new table with unique values, and I narrowed the rows down.

2. After cleaning the duplicated rows, I realized that in the sales_by_sku, I have a total of 462 rows, which means I should have 462 unique products as my primary key in sales_by_sku table. However, when I switch to look for the productsku in sales_report, I only see 454. There must be 8 products missing.

   
Queries:
Below, provide the SQL queries you used to clean your data.
1.
                  SELECT
                      visitId,
                      COUNT(*) AS duplicate_count
                  FROM
                      analytics
                  GROUP BY
                      visitId
                  HAVING
                      COUNT(*) > 1;
                  
                  --I tried multiple ways (Stackflow, Google), and (not yet) couldn't figure a way to                           normalize the table. Instead I created another table with DISTINCT, and deleted the                           duplication of visitId from 1000000 to 37887 rows.--

                  CREATE TABLE analytics_new AS
                  SELECT DISTINCT ON (visitId) *
                  FROM analytics;
                  
                  -- Drop the original table
                  DROP TABLE analytics;

                  -- Rename the new table to the original name
                  ALTER TABLE analytics_new RENAME TO analytics;

                  -- I did the same query with the duplicate fullvisitorIds, and further narrowed it down to                    34433 rows.

                  CREATE TABLE analytics_new AS
                  SELECT DISTINCT ON (fullvisitorId) *
                  FROM analytics;
                  DROP TABLE analytics;
                  ALTER TABLE analytics_new RENAME TO analytics;

                  -- I later did the same query with the productSKUs in products(here the column name is                        sku), sales_by_sku and sales_report table.


