What issues will you address by cleaning the data?
1. I recognized the duplications present in the tables, visitorId, fullVisitorId and ProductSKU. So I pulled the table out to identify them, then created new table with unique values, and I narrowed the rows down.

2. After cleaning the duplicated rows, I realized that in the sales_by_sku, I have a total of 462 rows, which means I should have 462 unique products as my primary key in sales_by_sku table. However, when I switch to look for the productsku in sales_report, I only see 454. There must be 8 products missing.

3. In analytics table, the visitstarttime column is in a different format, I need to change it.
   
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

                  
--I use this code to pull out the duplications that I need to clean. I did this with the fullvisitorId in all_sessions table as well. After this step, I created another table with DISTINCT, and deleted the              duplication of visitId from 1000000+ to 37887 rows in the analytics.--

                  CREATE TABLE analytics_new AS
                  SELECT DISTINCT ON (visitId) *
                  FROM analytics;
                  
-- Drop the original table

                  DROP TABLE analytics;

-- Rename the new table to the original name

                  ALTER TABLE analytics_new RENAME TO analytics;

-- I did the same query with the duplicate fullvisitorIds in all_sessions table, and further narrowed it down to 34433 rows in the analytics rable, and 534 rows in the all_sessions table.

                  CREATE TABLE 
		  	analytics_new AS
                  SELECT 
		  	DISTINCT ON (fullvisitorId) *
                  FROM 
		  	analytics;
                  DROP TABLE 
		  	analytics;
                  ALTER TABLE 
		  	analytics_new RENAME TO analytics;

-- I later did the same query with the productSKUs in products(here the column name is                        sku), sales_by_sku and sales_report table.


2.
--first I pulled out the missing 8 values from the sales_by_sku table:--
   
		SELECT
			DISTINCT sbs.productSKU
		FROM
  			sales_by_sku sbs
		LEFT JOIN
  			sales_report sr ON sbs.productSKU = sr.productSKU
		WHERE
  			sr.productSKU IS NULL;
   
--After I have retrieved these values, I used INSERT to put these 8 values into the sales_report table: I put 0s in so that I don't change the other column values, after this, I pulled the full sales_report again, it seems to work, but I don't have time to check. If I made a mistake, I might want to change the zeroes to NULL. Help me to pick the correct answer (or else) if I'm wrong, I'm really curious!
   
            INSERT INTO 
	    	sales_report (productSKU, total_ordered, stockLevel, restockingLeadTime, sentimentScore,                 	sentimentMagnitude, ratio)
            VALUES
                ('9180753', 0, 0, 0, 0.0, 0.0, 0.0),
                ('9182182', 0, 0, 0, 0.0, 0.0, 0.0),
                ('9182763', 0, 0, 0, 0.0, 0.0, 0.0),
                ('9182779', 0, 0, 0, 0.0, 0.0, 0.0),
                ('9184663', 0, 0, 0, 0.0, 0.0, 0.0),
                ('9184677', 0, 0, 0, 0.0, 0.0, 0.0),
                ('GGOEGALJ057912', 0, 0, 0, 0.0, 0.0, 0.0),
                ('GGOEYAXR066128', 0, 0, 0, 0.0, 0.0, 0.0);
3.

	    SELECT 
	        TO_CHAR(TO_TIMESTAMP(visitStartTime), 'YYYY-MM-DD HH24:MI:SS') 
	    AS
                readable_datetime
	    FROM
                analytics;
  
--It worked perfectly fine, however, after I tried to pull the tables out again, the format is still in its original form. I used ALTER:
  
           ALTER TABLE analytics
               ADD COLUMN formatted_visit_start_time TIMESTAMP;
	       
--However, I pulled the analytics table again, and received error feedback:
ERROR:  column "formatted_visit_start_time" of relation "analytics" already exists 
SQL state: 42701

--To prevent further complications, I had to do this:

               UPDATE analytics
               SET formatted_visit_start_time = TO_TIMESTAMP(visitStartTime);
               ALTER TABLE analytics
               
--Then same logic as deleting the duplicates, I dropped the original column:

               DROP COLUMN visitStartTime;
               ALTER TABLE analytics
               RENAME COLUMN formatted_visit_start_time TO visitStartTime;
               
--I received error message, unfortunately I decided to undo everything for the timstamp thing in order to avoid further complications, because I need to be able to pull my analytics table.
ERROR:  cached plan must not change result type 
SQL state: 0A000

--At this point I am able to pull "visitstarttime" from analytics table individually but couldn't pull the full analytics table out. I guess I should stop here.
