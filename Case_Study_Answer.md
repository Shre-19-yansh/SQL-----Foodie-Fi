## 🛤️ A. Customer Journey  

Based on the 8 sample customers from the `subscriptions` table, here is a brief description of each customer’s onboarding journey:  

---

1. **Customer_id 1**  
   - Started on a **trial plan** on *1st August 2020*  
   - Shifted to **basic monthly plan** on *8th August 2020*  

2. **Customer_id 2**  
   - Started on a **trial plan** on *20th September 2020*  
   - Shifted to **pro annual plan** on *27th September 2020*  

3. **Customer_id 11**  
   - Started on a **trial plan** on *19th November 2020*  
   - Churned (stopped using) on *26th November 2020*  

4. **Customer_id 13**  
   - Started on a **trial plan** on *15th December 2020*  
   - Shifted to **basic monthly plan** on *22nd December 2020*  
   - Upgraded to **pro monthly plan** on *29th March 2021*  

5. **Customer_id 15**  
   - Started on a **trial plan** on *17th March 2020*  
   - Shifted to **pro monthly plan** on *24th March 2020*  
   - Churned on *29th April 2020*  

6. **Customer_id 16**  
   - Started on a **trial plan** on *31st May 2020*  
   - Shifted to **basic monthly plan** on *7th June 2020*  
   - Upgraded to **pro annual plan** on *21st October 2020*  

7. **Customer_id 18**  
   - Started on a **trial plan** on *6th July 2020*  
   - Shifted to **pro monthly plan** on *13th July 2020*  

8. **Customer_id 19**  
   - Started on a **trial plan** on *22nd June 2020*  
   - Shifted to **pro monthly plan** on *29th June 2020*  
   - Upgraded to **pro annual plan** on *29th August 2020*  

---

## 📊 B. Data Analysis Questions  

---

1.	How many customers has Foodie-Fi ever had?
SQL
```SQL
SELECT COUNT(Distinct(customer_id)) As Total_Customer
FROM subscriptions
```
<img width="189" height="57" alt="image" src="https://github.com/user-attachments/assets/5cebf2fa-92f3-4ef9-9d52-b4acf86df870" />

 
2.	What is the monthly distribution of trial plan start_date values for our dataset - use the start of the month as the group by value ?
```SQL
SELECT start_month, COUNT(*)
FROM subscriptions
WHERE plan_id = 0
GROUP BY start_month
ORDER BY start_month
```
<img width="263" height="245" alt="image" src="https://github.com/user-attachments/assets/ed82c48d-9a44-44a3-9640-793e04076971" />

3.	What plan start_date values occur after the year 2020 for our dataset? Show the breakdown by count of events for each plan_name
```SQL
SELECT plan.plan_name, COUNT(*)
FROM subscriptions
JOIN plan
ON subscriptions.plan_id = plan.plan_id 
WHERE start_year > 2020
GROUP BY plan.plan_name
Order BY plan.plan_name
```
<img width="263" height="60" alt="image" src="https://github.com/user-attachments/assets/5933d4b8-9574-4564-84e4-9b9ceded9925" />

 
4.	What is the customer count and percentage of customers who have churned rounded to 1 decimal place?
```SQL
SELECT 
    COUNT(*) AS total_customers,
    SUM(CASE WHEN plan_id = 4 THEN 1 ELSE 0 END) AS churned_customers,
    ROUND(
        100.0 * SUM(CASE WHEN plan_id = 4 THEN 1 ELSE 0 END) / COUNT(*),
        1
    ) AS churn_percentage
FROM subscriptions;
```
<img width="525" height="64" alt="image" src="https://github.com/user-attachments/assets/f321be66-d01f-4917-b452-122388b54bc3" />

 
5.	How many customers have churned straight after their initial free trial - what percentage is this rounded to the nearest whole number?
```SQL
SELECT 
SUM(CASE WHEN plan_id = 4 then 1 ELSE 0 end) AS No_churned_customer,
ROUND(
    100.0 * SUM(CASE WHEN plan_id = 4 THEN 1 ELSE 0 END )/COUNT(*)
    ,0
) AS Perc_Churned_Customer
FROM subscriptions
```
<img width="451" height="57" alt="image" src="https://github.com/user-attachments/assets/6f4cc7b6-9df1-4ac8-8279-6aca48d80d70" />

 
6.	What is the number and percentage of customer plans after their initial free trial?
```SQL
WITH ordered_plans AS (
    SELECT 
        customer_id,
        plan_id,
        ROW_NUMBER() OVER (PARTITION BY customer_id ORDER BY start_date) AS rn
    FROM subscriptions
)
SELECT 
    plan_id AS first_paid_plan,
    COUNT(*) AS total_customers,
    ROUND(
        100.0 * COUNT(*) / (SELECT COUNT(DISTINCT customer_id) FROM subscriptions),
        1
    ) AS perc_customers
FROM ordered_plans
WHERE rn = 2   -- second plan (right after the free trial)
GROUP BY plan_id
ORDER BY plan_id;
```
<img width="472" height="142" alt="image" src="https://github.com/user-attachments/assets/7d23c77e-8890-4e68-8fc7-daade9ed181f" />

 
7.	What is the customer count and percentage breakdown of all 5 plan_name values at 2020-12-31?
8.	How many customers have upgraded to an annual plan in 2020?
```SQL
SELECT plan_id, COUNT(*) AS TOTAL 
FROM subscriptions
WHERE plan_id = 3 AND start_year = 2020
```
<img width="198" height="59" alt="image" src="https://github.com/user-attachments/assets/2911b73d-12c4-4fef-b81f-ff9399a4b9ce" />

9.	How many days on average does it take for a customer to an annual plan from the day they join Foodie-Fi?
```SQL
SELECT 
    AVG(DATEDIFF(annual_date, trial_date)) AS avg_days_to_annual
FROM (
    SELECT 
        s.customer_id,
        MIN(CASE WHEN s.plan_id = 0 THEN STR_TO_DATE(CONCAT(s.start_year, '-', s.start_month, '-', s.start_date), '%Y-%m-%d') END) AS trial_date,
        MIN(CASE WHEN s.plan_id = 3 THEN STR_TO_DATE(CONCAT(s.start_year, '-', s.start_month, '-', s.start_date), '%Y-%m-%d') END) AS annual_date
    FROM subscriptions s
    GROUP BY s.customer_id
) t
WHERE annual_date IS NOT NULL;
 ```
<img width="229" height="63" alt="image" src="https://github.com/user-attachments/assets/552d4771-7550-496b-970d-0497d2975464" />

10.	Can you further breakdown this average value into 30 day periods (i.e. 0-30 days, 31-60 days etc)
```SQL
SELECT 
    CASE 
        WHEN days_to_annual BETWEEN 0 AND 30 THEN '0-30 days'
        WHEN days_to_annual BETWEEN 31 AND 60 THEN '31-60 days'
        WHEN days_to_annual BETWEEN 61 AND 90 THEN '61-90 days'
        WHEN days_to_annual BETWEEN 91 AND 120 THEN '91-120 days'
        WHEN days_to_annual BETWEEN 121 AND 150 THEN '121-150 days'
        WHEN days_to_annual BETWEEN 151 AND 180 THEN '151-180 days'
        ELSE '180+ days'
    END AS period_bucket,
    COUNT(*) AS customer_count
FROM (
    SELECT 
        s.customer_id,
        DATEDIFF(
            MIN(CASE WHEN s.plan_id = 3 THEN STR_TO_DATE(CONCAT(s.start_year, '-', s.start_month, '-', s.start_date), '%Y-%m-%d') END),
            MIN(CASE WHEN s.plan_id = 0 THEN STR_TO_DATE(CONCAT(s.start_year, '-', s.start_month, '-', s.start_date), '%Y-%m-%d') END)
        ) AS days_to_annual
    FROM subscriptions s
    GROUP BY s.customer_id
    HAVING days_to_annual IS NOT NULL
) t
GROUP BY period_bucket
ORDER BY MIN(days_to_annual);
```
<img width="326" height="117" alt="image" src="https://github.com/user-attachments/assets/c9aa58e2-58f5-4e81-ace0-79c58365b23a" />

11.	How many customers downgraded from a pro monthly to a basic monthly plan in 2020?
```SQL
SELECT 
    COUNT(DISTINCT s1.customer_id) AS downgraded_customers
FROM subscriptions s1
JOIN subscriptions s2 
  ON s1.customer_id = s2.customer_id
WHERE s1.plan_id = 2
  AND s2.plan_id = 1
  AND STR_TO_DATE(CONCAT(s1.start_year, '-', s1.start_month, '-', s1.start_date), '%Y-%m-%d')
     < STR_TO_DATE(CONCAT(s2.start_year, '-', s2.start_month, '-', s2.start_date), '%Y-%m-%d')
  AND s1.start_year = 2020
  AND s2.start_year = 2020;
```
<img width="251" height="67" alt="image" src="https://github.com/user-attachments/assets/bbfa451a-3a4f-402a-ae16-26ea31c4987b" />

## 💡 D. Outside The Box Questions  

---

**1️⃣ How would you calculate the rate of growth for Foodie-Fi?**  
▢ I would calculate the rate of growth by:  
- 📈 Looking at how many customers are joining after their initial trial plan.  

---

**2️⃣ What key metrics would you recommend Foodie-Fi management to track over time to assess performance of their overall business?**  
▢ They should focus on:  
- 🔄 Their ability to retain customers  
- ⏳ Ensuring customers are part of the company for a long time  
- 📊 Measuring their growth over time  

---

**3️⃣ What are some key customer journeys or experiences that you would analyse further to improve customer retention?**  
▢ I would focus on:  
- 🎬 Improving and increasing the content available  
- 🤝 Strengthening customer service  

---

**4️⃣ If the Foodie-Fi team were to create an exit survey shown to customers who wish to cancel their subscription, what questions would you include in the survey?**  
▢ Questions should be:  
- ⭐ Their overall experience  
- ❓ Reason for exit  
- 🔧 Things to focus and improve  

---

**5️⃣ What business l**
▢ They could do this by:  
- 📚 Focusing on the quantity of the content available  
- 🎯 Focusing on the quality of the content available  
- ✅ Ensuring content passes the customer satisfaction level, so they stick around for a long time  


 



