# DataAnalytics-Assessment

## Overview
This repository contains SQL solutions for a data analysis assessment based on real-life user activity and transaction data. The focus is on customer behavior insights such as high-value users, transaction frequency, dormant accounts, and estimated lifetime value.

---

## ✅ Query Explanations 

### 🔹 Assessment_Q1.sql – High-Value Customers with Multiple Products
**What I Did:**
I identified customers who have at least one active savings account and one active investment plan. To define “active,” I checked for savings accounts where the confirmed amount is greater than 0, and plans where `is_a_fund = 1` and `amount > 0`.

**Challenge I Faced:**  
The `name` column in `users_customuser` was returning null values in several cases, so I used a fallback by concatenating the `first_name` and `last_name` columns to make sure the user name still displays correctly.

**Why This Works:**  
This query successfully returned 222 users who meet both savings and investment conditions. One key example is Obi David, who:

Has 8,964 savings transactions
Owns 24 investment plans
And has the highest total deposit value of NGN 9,351,183,458.72

While Obi David doesn’t have the highest number of investment plans overall, his combined activity and deposit value made him stand out clearly as a top customer. This validates that the query is identifying truly high-value users engaging across both products.

---

### 🔹 Assessment_Q2.sql – Transaction Frequency Analysis
**What I Did:**
I calculated how often each customer transacts on average per month using data from the savings_savingsaccount table. For each user, I counted their total transactions and divided by their tenure in months (difference between first and last transaction date). I then used a CASE statement to categorize each customer into one of the following:

High Frequency (10 or more transactions/month)
Medium Frequency (3–9 transactions/month)
Low Frequency (2 or fewer transactions/month)

**Why This Works:** 
This logic helps us group customers based on their level of activity. From the results:

154 customers are High Frequency users with an average of 48.4 transactions/month
116 customers fall into the Medium Frequency group, averaging 4.9 transactions/month
603 customers are Low Frequency, with just 1.4 transactions/month 

These insights are helpful for customer segmentation - e.g., targeting high-frequency userss for loyalty programs, or re-engaging low-frequency users with offers or nudges

---

### 🔹 Assessment_Q3.sql – Account Inactivity Alert
**What I Did:**
I created a query that flags accounts (both savings and investments) that haven’t had any deposit (confirmed_amount) in the last 365 days. I used DATEDIFF to compare today’s date with the most recent transaction for each plan. I grouped by the plan and owner to get the latest transaction date and calculate inactivity.

**Why This Works:**
By checking if a plan has gone over a year without any deposit transactions, this query helps identify inactive accounts that may need follow-up. It's especially useful for operations or customer success teams to re-engage dormant users or flag them for review.

---

### 🔹 Assessment_Q4.sql – Customer Lifetime Value (CLV) Estimation
**What I Did:**
I estimated the CLV of each user using the given formula:  
> **CLV = (transactions/month) × 12 × 0.001 × (total transaction value)**

To do this, I:
- Counted total savings transactions
- Calculated user tenure in months from 'date_joined'
- Summed their confirmed transaction amounts
- Applied the formula and ranked the results

**Why This Works:**  
It gives a rough but useful estimate of each customer’s future potential value to the business, helping prioritize engagement strategies.

---

## 🧠 Challenges Faced
- **Null names**: In Q1 and Q4, I noticed `name` values were often missing, so I fixed it by combining `first_name` and `last_name`.
- **Zero-division errors**: In Q2 and Q4, I used `NULLIF()` in the denominator to safely avoid dividing by 0.
- **Handling multiple product types**: It was important to distinguish between savings and investment products using fields like `is_a_fund`, `is_regular_savings`, and plan amounts.
- **DateTime Format**: In Q3, after running my initial query, the last_transaction_date included both date and time, which wasn’t needed for the report.
To fix this, I modified the query and used the DATE() function to extract only the date portion — so now it just shows something like 2025-05-18 instead of 2025-05-18 18:23:00.
