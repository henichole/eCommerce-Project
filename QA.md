What are your risk areas? Identify and describe them.

My risk areas include:

The visitID column in the "all_sessions" table doesn't align with the fullvisitorID in the "analytics" table due to duplications. I'll require additional time to investigate and understand the differences and their true significance.

The "transactionrevenue" section in the "all_sessions" table contains a significant number of null values, which may impact the analysis.

Various issues are present, such as errors in city names, time format discrepancies, and duplications in productSKUs, among other challenges. The list of data quality issues seems extensive and could require further attention and cleaning.


QA Process:
Describe your QA process and include the SQL queries used to execute it.

Remove duplicated rows with the same productSKU in the five tables, identifying and resolving the 8 duplicated products.

Eliminate duplicated rows with the same fullvisitorId, visitorID in the "all_sessions" and "analytics" tables.

Divide the unit price by 1000000 to adjust the value.

Update default values to "null" for relevant fields.

Convert UNIX timestamps in the "visitstarttime" column to readable timestamps.

Investigate the meaning of sentiment scores and sentiment magnitude, and analyze the resulting data using SQL.
Identify the product with the highest order number, low stock, and longer restocking lead time.

Analyze trends in different products sold globally and explore potential impacts.

Summarize your findings and create a presentation with graphical visualizations to convey the results effectively.
