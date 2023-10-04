What issues will you address by cleaning the data?

1.When I rank the total revenue, orders placed in descending order
for the total revenue, a lot of the totaltransactionrevenue, transactions, names of the cities datas are missing.

2. I get a list of null values on the revenue side, meaning that the revenue column is not useful in the all_sessions table. I can use COALESCE to replace the null with "N/A". To encourage potential future data improvement. I can also use COALESCE to replace the null valus with "N/A" to flter out rows with null values using WHERE clause:

3. The prices are not making any sense, I have to SET product_price = all_sessions.productPrice / 1000000;

4. What on earth is going on with all kinds of IDs in different tables? Columns I see in the all_sessions table: fullvisitorID (17-18 digits ID number). Columns I see in the analytics table: visitNumber (SMALLINT of 2 digits)
and visitID(10 digits ID number). I joined values in different columns from different tables that are the same.
found out that the top customer with visitID is 


Queries:
Below, provide the SQL queries you used to clean your data.
SQL1:
      SELECT
          country,
          city,
          totalTransactionRevenue AS revenue,
          ROW_NUMBER() OVER (ORDER BY totalTransactionRevenue DESC) AS revenue_rank
      FROM
          all_sessions;
      for the order placed:
      SELECT
          country,
          city,
          transactions AS orders_placed,
          ROW_NUMBER() OVER (ORDER BY transactions DESC) AS orders_rank
      FROM
          all_sessions;
Top 3 countries with the most orders: US, India and Singapore. Since the first and third place are both the US without a city, I aggregated the rows to give US the first place.

SQL2:
      SELECT
      	country,
          city,
          COALESCE(totaltransactionrevenue, 'N/A') AS revenue
      FROM
          all_sessions
      WHERE 
          totaltransactionrevenue IS NULL;
SQL3:
      UPDATE all_sessions
      SET productprice = all_sessions.productprice / 1000000;



