What issues will you address by cleaning the data?
1. Recognize the duplications present in analytics table
2. Normalize them.
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
                  
--I tried multiple ways (Stackflow, Google), and (not yet) couldn't figure a way to normalize the table. Instead I created another table with DISTINCT, and deleted the duplication of visitId from 1000000 to 37887 rows.--

                  CREATE TABLE analytics_new AS
                  SELECT DISTINCT ON (visitId) *
                  FROM analytics;
                  
                  -- Drop the original table
                  DROP TABLE analytics;

                  -- Rename the new table to the original name
                  ALTER TABLE analytics_new RENAME TO analytics;

-- I did the same query with the duplicate fullvisitorIds, and further narrowed it down to 34433 rows.

                  CREATE TABLE analytics_new AS
                  SELECT DISTINCT ON (fullvisitorId) *
                  FROM analytics;
                  DROP TABLE analytics;
                  ALTER TABLE analytics_new RENAME TO analytics;

-- I'm not going to copy paste all of my queries here, I later did the same query with the productSKUs in all the tables.

