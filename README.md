# Customer-Shopping-Behaviour-Analysis-Simulated-Dataset-
An end-to-end data analysis project exploring retail customer behaviour using Python, PostgreSQL, and Power BI - uncovering spending patterns, customer segments, product preferences, and subscription trends to guide strategic business decisions.
📌 Project Overview

This project analyzes transactional data from 3,900 purchases across multiple product categories to answer key business questions around customer demographics, purchasing behavior, and revenue drivers. The pipeline covers data cleaning and feature engineering in Python, structured querying in PostgreSQL, and visualization through an interactive Power BI dashboard.

📊 Dataset Summary
Attribute	Details
Rows	3,900
Columns	18
Missing Data	37 values in review_rating

Key Features

Customer demographics: Age, Gender, Location, Subscription Status
Purchase details: Item Purchased, Category, Purchase Amount, Season, Size, Color
Shopping behavior: Discount Applied, Promo Code Used, Previous Purchases, Frequency of Purchases, Review Rating, Shipping Type
🧹 Data Preparation & Feature Engineering (Python)
Data Loading: Imported dataset using pandas; explored structure with df.info() and .describe()
Missing Data Handling: Imputed missing review_rating values using the median rating per product category
python
  df["Review Rating"] = df.groupby("Category")["Review Rating"].transform(lambda x: x.fillna(x.median()))
Column Standardization: Converted all column names to snake_case for readability
Feature Engineering:
age_group — binned customer ages into Young Adult / Adult / Middle-aged / Senior using pd.cut() (chosen over pd.qcut() to reflect real population distribution rather than introduce statistical bias)
purchase_frequency_days — mapped purchase frequency labels (Weekly, Monthly, etc.) to numeric day intervals
Data Consistency Check: Verified discount_applied and promo_code_used were redundant (100% match) and dropped promo_code_used
Database Integration: Loaded the cleaned DataFrame into PostgreSQL via SQLAlchemy + psycopg2 for structured SQL analysis
🗃️ SQL Analysis (PostgreSQL)

Ten structured queries were run to answer key business questions:

#	Analysis	Key Insight
1	Revenue by Gender	Male customers generated $157,890 vs. $75,191 from female customers
2	High-Spending Discount Users	839 of 3,900 customers (21.5%) used discounts while spending above average
3	Top 5 Products by Rating	Gloves, Sandals, Boots, Hat, and Skirt led average review ratings
4	Shipping Type Comparison	Express shipping customers spend slightly more on average ($60.48 vs. $58.46)
5	Subscribers vs. Non-subscribers	Non-subscribers actually out-spend subscribers on average ($59.87 vs. $59.49)
6	Discount-Dependent Products	Hat, Sneakers, Coat, Sweater, and Pants are discounted in ~47–50% of purchases
7	Customer Segmentation	Classified into New (83), Returning (701), and Loyal (3,116) using a CTE
8	Top 3 Products per Category	Ranked using ROW_NUMBER() window function partitioned by category
9	Repeat Buyers & Subscriptions	Only 958 of 3,476 repeat buyers (>5 purchases) are subscribed
10	Revenue by Age Group	Middle-aged and Adult segments drive ~72% of total revenue

Bonus — Bayes' Theorem: Calculated P(Subscriber | Repeat Buyer) = 0.074, showing that being a repeat buyer barely moves the likelihood of subscribing.

Technical notes:

Explicitly cast review_rating to numeric in SQL, since the Python-to-PostgreSQL load typecasted it as double precision, which cannot be rounded directly
Used ROW_NUMBER() instead of RANK() / DENSE_RANK() for per-category rankings, since it assigns a unique rank even to tied totals
📈 Power BI Dashboard

An interactive dashboard was built with slicers for Subscription Status, Gender, Category, and Shipping Type, featuring:

KPI cards: 3.9K customers | $59.76 avg. purchase | 3.75 avg. review rating
Subscription status breakdown (27% Yes / 73% No)
Revenue and sales by category
Revenue and sales by age group
💡 Business Recommendations
Boost Subscriptions — Promote exclusive, higher-value benefits for subscribers
Customer Loyalty Programs — Reward repeat buyers to convert them into the "Loyal" segment
Review Discount Policy — Balance sales boosts against margin erosion on heavily discounted items
Product Positioning — Highlight top-rated and best-selling products in marketing campaigns
Targeted Marketing — Focus efforts on high-revenue age groups and express-shipping users
🛠️ Tech Stack
Python — pandas (data cleaning, feature engineering)
PostgreSQL — SQLAlchemy, psycopg2 (structured business-question analysis)
Power BI — interactive dashboard and visualization
