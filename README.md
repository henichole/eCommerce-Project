# Final-Project-Transforming-and-Analyzing-Data-with-SQL

## Project/Goals
The goal of this project is to do QA of the five given data tables, which include practices such as normalization and intepretation. At the end of the project, I want to present to my audience as if they are my business partners, for example, I would explain what I have found and give advise on what to improve in the future.

## Process
Step one: 
I started with the five given questions, because other than starring at the tables and think about what to do as a complete beginner, it would be easier for me to tackle the technical questions first. 

Step two: 
After solving the five questions, I started to notice the issues, and then I went back to the basics of QA practices. I felt much easier doing that since I have already tackled the harder portion of this project. Some of the columns are too empty to tell a story. For example, in the first question, I was required to find the transaction value and trend of orders and products from certain areas. However, I noticed that many of the transactions with orderIDS had zero revenue, this did not make sense to me. Therefore, I moved to the TotalTransactionRevenue column and used that one instead. 

Step three:
While debugging each issues one by one, I started the true QA process by cleaning the duplications, errors, and combined using JOIN and UNION to find relevant and useful trends to present.

Step four:
Wrap up with some issues.

## Results
1. Most of the ordered were made via organic search, direct and referral.
2. The top counties that had most orders are: United Stated, Kuwait, Indonesia and Germany.
3. The city that had the highest average of orders is Austin, Kalamazoo and San Antonio.
4. The city that made the most money in transaction revenue was Chicago US and Toronto CA, but since there are a lot of null values in the US, the results could be incorrect.
5. The top five selling products are ballpoint LED Light Pen, 17oz Stainless Steel Sport Bottle, Leatherette Journal, Spiral Journal with Pen and Foam Can and Bottle Cooler.
6. I do not find a correlation between the unique "fullvisitorId" versus the "productviewed" and "totaltransactionrevenue" columns, since most the of the "transactionrevenue" and "totaltransactionrevenue" columns are missing values from the dataset, according to starting_with_data file.
7. I found 15 products with identical sentiment score of 0.9 and sentiment magnitude of 1.4: Some of these are listed below: "Recycled Paper Journal Set" "Women's Long Sleeve Blended Cardigan Charcoal".

## Challenges 
Day 1-2:
As complete beginner, I was stupid enough to forget everything I learned in the PrepCourse and did not realize that part of the requirements are right here on Github. Therefore, without the tutorials and guidelines, I thought I had to do this completely out of scratch and I spent two full days trying to port in the five data tables. I manually inserted columns to tables, one by one using sql queries. Overall, it was not a waste of time, because I learned to clean the data format on Excel, and mastered the "INSERT" and some other clauses in postgresql. 

Day 3-4:
I realized that I am more confused than the table than learning to write queries. I had to Google some of the columns to find out the meanings, for example, the sentimentscores and ratio. Luckily, this project is supposed to be challenging for us to not have enough time, or else I would defenitely dig in deeper because I got annoyed by the fact that I had to analyze a set of data which I knew nothing of - I would have to practice in the future as well, since this would be my new norm.

Technical Challenges:
Since I made mistakes in day 1-2 when trying to port in the tables in, I didn't specify the column names and data types correctly. I learned my lessons later, casting and altering my column names and data types, which was an extra thing to do, but good practice for myself.

## Future Goals
1. As mentioned above, next time I would want to clean the datas first, since I jumped back and fourth with this one, which was a "waste of time" for my future self. But it was a good practice for now since I enjoyed the debugging process! 
2. I would have to practice using open sources and online tools more often, other than sweating myself typing & importing queries and datas manually.
3. My potential errors could be in the "visitId" and "fullvisitorId" columns in the all_sessions and analytics table. I tried my best within the provided time to clean these off a little bit, but I'm still finding my results a bit ambiguous.
4. I want more time to dig into the "time spent on site", "sentimental scores" and "sentimental magnitude" columns in the sales_report table.
5. More experience needed for sure, I am only scratching the surface here, I hope that I can have more time, or more relevant examples to look through in the future.
   
