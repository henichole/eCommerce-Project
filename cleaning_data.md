What issues will you address by cleaning the data?
1.Join duplicate columns
2.Rank the revenue, orders placed in descending order
3.Clear null values


5. Change the unit price by /1000000

UPDATE all_sessions
SET product_price = all_sessions.productPrice / 1000000;

6. What on earth is going on with all kinds of IDs in different tables?
   Columns I see in the all_sessions table:
      fullvisitorID (17-18 digits ID number)
   COlumns I see in the analytics table:
      visitNumber (SMALLINT of 2 digits)
      visitID(10 digits ID number)



Queries:
Below, provide the SQL queries you used to clean your data.


