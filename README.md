# 🍜 Foodie-Fi – SQL Challenge 3  
<img width="419" height="421" alt="image" src="https://github.com/user-attachments/assets/b04de351-6478-4b6d-a0e8-c6f549fe7c1e" />


## 📖 Project Overview  
This project is part of the **8 Week SQL Challenge**, focusing on the **Foodie-Fi case study**.  

The challenge simulates a **subscription-based video streaming startup** offering food-only content (like Netflix, but with cooking shows). The aim is to analyze subscription data to uncover customer journeys, retention patterns, churn behavior, and revenue impact.  

The main goal is to prepare and analyze the data to gain insights into:  

- 🍜 Customer journeys and onboarding paths  
- 📊 Subscription patterns across different plans  
- 🚪 Churn behavior and customer retention  
- 📈 Business growth through upgrades and annual plans  

---

## 🎯 Objective  
- 🧹 Clean and transform raw data for better analysis  
- 📆 Split `start_date` into **day, month, and year** for easier aggregations  
- 🔗 Combine and query datasets to analyze customer journeys  
- 💡 Generate insights into churn, upgrades, downgrades, and revenue trends  

---

## 📂 Files in This Repository  
- `plans.csv` → Subscription plan details (Basic, Pro, Annual, Churn)  
- `subscriptions.csv` → Customer subscription history with start dates  
- `case-study-solution.sql` → SQL queries used for analysis  
- `README.md` → Project documentation (this file)  

---

## 🧹 Data Preparation  
During the data preparation process, the following tasks were performed:  

- 📆 **Splitting Date Columns** – Transformed `start_date` into separate **date, month, year** columns for time-based aggregations  
- 🔗 **Joins & Relationships** – Used the entity relationship between `plans` and `subscriptions` for analysis  
- 📊 **Tracking Journeys** – Followed customers from trial to different subscription paths, churn, or upgrades 

---

## 📊 Expected Outcomes  
✅ Clear understanding of customer journeys from trial to different plans  
📈 Insights into upgrades, downgrades, churn, and annual subscriptions  
💡 Identification of key metrics to monitor business growth and retention  
🚀 Recommendations to improve customer experience and reduce churn  

---

## 🔄 Process  
1. **Understand the schema** – Reviewed ERD and table structures (`plans`, `subscriptions`).
   <img width="391" height="136" alt="image" src="https://github.com/user-attachments/assets/9cada03c-9920-40fe-9ae9-c851ae7b88c2" />
3. **Identify data needs** – Focused on subscription journeys, churn rates, upgrades, and payments.  
4. **Prepare the data** – Split dates, ensured consistent formats.  
5. **Analyze with SQL** – Wrote queries to generate insights into customer behavior and business performance.  
6. **Document findings** – Summarized results and insights in structured case study solutions.  

---

