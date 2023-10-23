What issues will you address by cleaning the data?
I identified duplications in the tables, specifically in the columns visitorId, fullVisitorId, and ProductSKU. To address this issue, I initially extracted the table to pinpoint these duplicates. Subsequently, I created a new table containing only unique values and filtered down the rows.

After removing the duplicated rows, I noticed that in the sales_by_sku table, there were a total of 462 rows. This implied that there should be 462 unique products serving as the primary key in the sales_by_sku table. However, when I checked the productsku in the sales_report, I only found 454 unique products. This suggests that there are 8 products missing.

In the analytics table, I observed that the visitstarttime column was in a different format, necessitating a format change.
   
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

                  
--I utilized the following code to identify and extract the duplications that required cleaning. I performed a similar operation with the fullVisitorId in the all_sessions table. Subsequently, I created another table using the DISTINCT keyword and eliminated the redundant rows, reducing the visitId entries from over 1,000,000 to 37,887 rows in the analytics table.

                  CREATE TABLE analytics_new AS
                  SELECT DISTINCT ON (visitId) *
                  FROM analytics;
                  
-- Drop the original table

                  DROP TABLE analytics;

-- Rename the new table to the original name

                  ALTER TABLE analytics_new RENAME TO analytics;

-- I did the same query with the duplicated fullvisitorIds in all_sessions table.

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

-- Finally, I executed the same query on the productSKUs, where the column name is 'sku,' in the products, sales_by_sku, and sales_report tables.


2.
--Initially, I extracted the missing 8 values from the sales_by_sku table:
   
		SELECT
			DISTINCT sbs.productSKU
		FROM
  			sales_by_sku sbs
		LEFT JOIN
  			sales_report sr ON sbs.productSKU = sr.productSKU
		WHERE
  			sr.productSKU IS NULL;
   
--After retrieving these values, I used the INSERT statement to add these 8 values to the sales_report table. I inserted 0s to avoid altering the values in the other columns. Following this, I pulled the full sales_report dataset again. If there was an error or mistake, and you are unsure, you might consider changing the zeroes back to NULL. Reviewing the data and ensuring its accuracy is a good practice. If you suspect any issues or discrepancies, it's a good idea to verify the data to maintain data integrity.
   
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
		
3.Changing UNIX timestamp

	    SELECT 
	        TO_CHAR(TO_TIMESTAMP(visitStartTime), 'YYYY-MM-DD HH24:MI:SS') 
	    AS
                readable_datetime
	    FROM
                analytics;
  
--It's great to hear that your previous steps worked perfectly. If you used the ALTER statement to change the format of the table but didn't see the expected changes, you might want to double-check the syntax and make sure the ALTER statement was executed correctly. Ensure that you specified the column and the new format correctly in your ALTER statement.

If you encounter any issues or if you'd like to share the specific ALTER statement you used, I can help you further in diagnosing the problem or suggesting a solution.
  
           ALTER TABLE analytics
               ADD COLUMN formatted_visit_start_time TIMESTAMP;
	       
--However, I pulled the analytics table again, and received error feedback:
ERROR:  column "formatted_visit_start_time" of relation "analytics" already exists 
SQL state: 42701

--To prevent further complications, do this:

               UPDATE analytics
               SET formatted_visit_start_time = TO_TIMESTAMP(visitStartTime);
               ALTER TABLE analytics
               
--Drop the original column:

               DROP COLUMN visitStartTime;
               ALTER TABLE analytics
               RENAME COLUMN formatted_visit_start_time TO visitStartTime;
               
--I received an error message. Unfortunately, I decided to revert all the changes related to the timestamp to prevent further complications, as I need to be able to retrieve my analytics table.
ERROR:  cached plan must not change result type 
SQL state: 0A000

--At this point, I can retrieve "visitstarttime" from the analytics table individually, but I'm encountering difficulties when trying to extract the entire analytics table. I believe it might be best to halt further attempts for now.

--Update on Oct 4t: The timestamp is showing in its correct format today, good news.
