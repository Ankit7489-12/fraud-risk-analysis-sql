# fraud-risk-analysis-sql
Advanced SQL project for fraud detection and financial risk analysis

# 🧠 Fraud & Risk Analysis SQL Project

This project simulates a real-world consulting scenario in a financial institution, focusing on detecting fraudulent transactions and analyzing customer risk using advanced SQL techniques.

---

## 📁 Project Files

| File | Description |
|------|-------------|
| `sql_schema_creation.sql` | SQL script to create schema: customers, accounts, transactions, suspicious_flags |
| `db_customers.csv` | Customer data including demographics and account type |
| `db_accounts.csv` | Account information (status, type, open date) |
| `db_transactions.csv` | Historical credit and debit transactions |
| `db_suspicious_flags.csv` | Flags for accounts with suspicious behavior |

---

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
