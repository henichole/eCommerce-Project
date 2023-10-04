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
The visitor who has an Id of 1957460000000000000, unfortunately, I use a mac, and the analytics.csv file is too big for Numbers application on Mac to open, when I open in Excel, Excel automatically rounded this number to 6 sig figs. I tried to solve this problem, but still the best pract 


Question 2: 

SQL Queries:

Answer:



Question 3: 

SQL Queries:

Answer:



Question 4: 

SQL Queries:

Answer:



Question 5: 

SQL Queries:

Answer:
