# Final-Project-Transforming-and-Analyzing-Data-with-SQL

## Project/Goals

The objective of this project is to perform a comprehensive Quality Assurance (QA) analysis of the five provided data tables. This analysis will encompass various practices, including data normalization and interpretation. The ultimate goal is to prepare a presentation for my audience, who will be viewed as my business partners. In this presentation, I will explain the findings, share insights, and provide recommendations for potential improvements in the future. This approach will not only ensure data accuracy but also support informed decision-making and business growth.

## Process
Step one: I started with the five given questions because, as a complete beginner, it's easier for me to tackle the technical questions first, rather than staring at the tables and contemplating what to do.

Step two: After solving the five questions, I began to notice issues in the data. I then returned to the fundamentals of QA practices. This approach felt much easier since I had already tackled the more challenging part of the project. I observed that some columns contained insufficient data to convey a meaningful story. For instance, in the first question, I was required to find transaction values and trends for orders and products from specific areas. However, I noticed that many transactions with orderIDs had zero revenue, which seemed illogical. As a result, I shifted my focus to the "TotalTransactionRevenue" column, using it as a more relevant alternative.

Step three: While resolving issues one by one, I initiated the actual QA process. I began by cleaning duplications, addressing errors, and employing JOIN and UNION operations to uncover pertinent and valuable trends for the presentation.

Step four:
Wrap up.

## Results
Most of the orders were made through organic search, direct, and referral sources.

The top countries with the most orders are the United States, Kuwait, Indonesia, and Germany.

The cities with the highest average of orders include Austin, Kalamazoo, and San Antonio.

The cities that generated the most transaction revenue were Chicago in the United States and Toronto in Canada. However, it's important to note that there are many null values in the United States, so the results might not be entirely accurate.

The top five selling products in my analysis are the ballpoint LED Light Pen, 17oz Stainless Steel Sport Bottle, Leatherette Journal, Spiral Journal with Pen, and Foam Can and Bottle Cooler.

I didn't find a clear correlation between the unique "fullvisitorId" and the "productviewed" and "totaltransactionrevenue" columns. This is likely due to the large number of missing values in the "transactionrevenue" and "totaltransactionrevenue" columns, as observed in the starting_with_data file.

I identified 15 products with identical sentiment scores of 0.9 and sentiment magnitudes of 1.4. Some of these products include "Recycled Paper Journal Set" and "Women's Long Sleeve Blended Cardigan Charcoal."

## Challenges 
Day 1-2:
As a complete beginner, I initially forgot most of what I had learned in the PrepCourse. Not realizing that some of the requirements were available on Github, I attempted to tackle the project entirely from scratch. I spent two full days manually inserting columns into the tables using SQL queries. Although this process was time-consuming, it wasn't a waste of time. It helped me refine my data formatting skills in Excel and allowed me to master the use of "INSERT" and various other SQL clauses.

Day 3-4:
I eventually recognized that I felt more confused by the data itself than by the process of writing queries. I needed to resort to Google to understand the meanings of certain columns, like "sentimentscores" and "ratio." Fortunately, this project is designed to be challenging and time-constrained, preventing me from delving too deeply into each aspect. I experienced some frustration as I had to analyze a dataset about which I knew very little. This experience served as a reminder that ongoing practice is necessary in the journey of learning, as this would likely become the new norm.

Technical Challenges:
I made mistakes during days 1-2 when attempting to import the tables. I didn't specify the column names and data types correctly. However, I learned from these mistakes and later on, I took the necessary steps to cast and alter my column names and data types. While it added extra work, it turned out to be a valuable learning experience and good practice for me.

## Future Goals
As mentioned earlier, for future projects, I would prioritize cleaning the data first before diving into analysis. Jumping back and forth between tasks was somewhat inefficient, but it served as valuable practice for debugging.

I recognize the need to practice using open-source tools and online resources more frequently. Relying on manual input and typing queries can be time-consuming, and I could benefit from exploring alternative methods.

I encountered potential challenges with the "visitId" and "fullvisitorId" columns in the "all_sessions" and "analytics" tables. I made efforts to clean and understand them within the given time frame, but the results remained somewhat ambiguous. This highlights the importance of further investigation and refinement.

I would like more time to explore and analyze the "time spent on site," "sentiment scores," and "sentiment magnitude" columns in the "sales_report" table, as these seem to hold significant insights.

It's clear that I need more experience in data analysis. I'm only scratching the surface, and I hope to have the opportunity to gain more experience or work on more relevant examples in the future.
