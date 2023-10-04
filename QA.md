What are your risk areas? Identify and describe them.

My risk areas include:

1. Due to my QA process, I cleaned the data of analytics and all_sessions table, due to the differences between the number of rows in these two tables (~30000 rows in analytics, and ~500 rows in all_sessions). The number visitID does not match the fullvisitorID, and I need more time to figure out the differences and true meanings of these two.
   
2. There are too many null values in the transactionrevenue section from the all_sessions table.

3. Errors in the city names, time formats, and duplications in productSKUs, the lists goes on.


QA Process:
Describe your QA process and include the SQL queries used to execute it.

1. Clean out the duplications rows with same productSKU since this is the first thing I could understand from these five tables.

2.
