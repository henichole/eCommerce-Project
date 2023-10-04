What are your risk areas? Identify and describe them.

My risk areas include:

1. Due to the duplications in many values, the column visitID in "all_sessions" table does not match the fullvisitorID in "analytics" table, and I need more time to figure out the differences and true meanings of these two.  
2. There are too many null values in the transactionrevenue section from the all_sessions table.
3. Errors in the city names, time formats, and duplications in productSKUs, the lists goes on.


QA Process:
Describe your QA process and include the SQL queries used to execute it.

1. Clean out the duplicated rows with same productSKU since this is the first thing I could understand from these five tables. I was able to find 8 duplicated products. 
2. Clean out the duplicated rows with same fullvisitorId, visitorID in all_sessions and analytics table
3. Divide the unit price by 1000000.
4. Update the default value to "null" values.
5. Update the UNIX timestamp of "visitstarttime" to readable timestamp.
6. Study the meaning of sentiment scores and setiment magnitude, and try to make sense of the resulting data via SQL.
7. Find the product with the highest order number, but lower stock and longer restocking leading time.
8. After solving my own questions, find the trends in different products sold globally, and whethere there is an impact.
9. Wrap up and make the presentation using graphical visualizations.
