# fraud-risk-analysis-sql
Advanced SQL project for fraud detection and financial risk analysis

-- --------------------------------------------
-- 🧠 Fraud & Risk Analysis SQL Project
-- Schema Creation Script
-- Author: Ankit Chanpuriya
-- Description: Creates the tables required for analyzing fraud patterns and customer risk
-- --------------------------------------------

-- Drop tables if they already exist (optional for reruns)
DROP TABLE IF EXISTS suspicious_flags;
DROP TABLE IF EXISTS transactions;
DROP TABLE IF EXISTS accounts;
DROP TABLE IF EXISTS customers;

-- ---------------------------
-- 1. Customers Table
-- ---------------------------
CREATE TABLE customers (
    customer_id INT PRIMARY KEY,
    name VARCHAR(100),
    age INT,
    gender VARCHAR(10),
    city VARCHAR(100),
    state VARCHAR(100),
    account_type VARCHAR(50)
);

-- ---------------------------
-- 2. Accounts Table
-- ---------------------------
CREATE TABLE accounts (
    account_id INT PRIMARY KEY,
    customer_id INT,
    account_open_date DATE,
    account_status VARCHAR(20),
    FOREIGN KEY (customer_id) REFERENCES customers(customer_id)
);

-- ---------------------------
-- 3. Transactions Table
-- ---------------------------
CREATE TABLE transactions (
    transaction_id INT PRIMARY KEY,
    account_id INT,
    amount DECIMAL(12, 2),
    transaction_type VARCHAR(10), -- 'credit' or 'debit'
    timestamp TIMESTAMP,
    location VARCHAR(100),
    FOREIGN KEY (account_id) REFERENCES accounts(account_id)
);

-- ---------------------------
-- 4. Suspicious Flags Table
-- ---------------------------
CREATE TABLE suspicious_flags (
    flag_id INT PRIMARY KEY,
    account_id INT,
    flag_type VARCHAR(100),
    flag_description TEXT,
    flag_date DATE,
    FOREIGN KEY (account_id) REFERENCES accounts(account_id)
);


✅ 1. High-Value Debit Transactions (> ₹50,000)
Find accounts with frequent high-value debits.

SELECT 
    account_id,
    COUNT(*) AS high_value_txns,
    SUM(amount) AS total_amount
FROM transactions
WHERE transaction_type = 'debit' AND amount > 50000
GROUP BY account_id
HAVING COUNT(*) > 3;

🌍 2. Multiple Locations in a Day (Possible Fraud)
Identify accounts that made transactions from more than 2 cities on the same day.

SELECT 
    account_id,
    DATE(timestamp) AS txn_date,
    COUNT(DISTINCT location) AS location_count
FROM transactions
GROUP BY account_id, DATE(timestamp)
HAVING COUNT(DISTINCT location) > 2;


📉 3. Inactive Accounts Suddenly Active with Large Transactions
Catch closed/inactive accounts making large debits.

SELECT 
    a.account_id,
    a.status,
    t.transaction_id,
    t.amount,
    t.timestamp
FROM accounts a
JOIN transactions t ON a.account_id = t.account_id
WHERE a.status IN ('Inactive', 'Closed') AND t.amount > 20000;


⚠️ 5. Risk Score for Each Customer
Assign a risk level based on transaction volume and flag count.

SELECT 
    c.customer_id,
    c.name,
    COUNT(DISTINCT f.flag_type) AS total_flags,
    COUNT(t.transaction_id) AS txn_count,
    AVG(t.amount) AS avg_txn_amount,
    CASE
        WHEN COUNT(DISTINCT f.flag_type) > 1 OR AVG(t.amount) > 40000 THEN 'High Risk'
        WHEN COUNT(DISTINCT f.flag_type) = 1 THEN 'Medium Risk'
        ELSE 'Low Risk'
    END AS risk_level
FROM customers c
JOIN accounts a ON c.customer_id = a.customer_id
LEFT JOIN transactions t ON a.account_id = t.account_id
LEFT JOIN suspicious_flags f ON a.account_id = f.account_id
GROUP BY c.customer_id, c.name;


💼 6. Flag Summary by Region
Useful for internal audit/regulatory compliance.

SELECT 
    c.location,
    COUNT(DISTINCT f.account_id) AS flagged_accounts
FROM suspicious_flags f
JOIN accounts a ON f.account_id = a.account_id
JOIN customers c ON a.customer_id = c.customer_id
GROUP BY c.location
ORDER BY flagged_accounts DESC;



## 🧠 Key SQL Skills Demonstrated

- Complex JOINs across multiple tables  
- Aggregations with `GROUP BY`, `HAVING`, `SUM`, `AVG`  
- Conditional logic using `CASE` statements  
- Date/time operations for trend analysis (`DATE_TRUNC`, `DATE_FORMAT`)  
- Risk scoring logic based on transaction behavior  
- Location-wise fraud analysis  
- Flag-based filtering and segmentation  

---

## 💼 Business Use Case

A bank wants to:

- Detect accounts with high-value or high-frequency debit transactions  
- Identify locations where transactions from the same account happen in multiple cities in one day  
- Monitor transactions from flagged accounts over time  
- Classify customers into Low, Medium, or High Risk based on spending patterns and flagged activity  

---


