# Case Study: Retail Analytics 101 with PySpark

## Overview
This case study provides a realistic yet compact **retail dataset** designed for basics of **PySpark** using **Databricks Free**.  
It simulates data from a small retail chain with multiple stores, products, and customers. Students learn how to load, clean, transform, and analyse data using Spark DataFrames.

---

## Dataset Description

### 1. Customers
- **Rows:** 250  
- **Fields:** customer_id, first_name, last_name, email, signup_date, age, loyalty_tier, is_student, home_city, home_region  
- Represents customer demographics and loyalty levels.

### 2. Products
- **Rows:** 60  
- **Fields:** product_id, product_name, category, brand, unit_price_list, tax_rate  
- Contains product catalog information with prices, categories, and brands.

### 3. Stores
- **Rows:** 12  
- **Fields:** store_id, store_name, city, region, opened_date, store_type  
- Describes the different stores in various regions (urban, mall, outlet, etc.).

### 4. Transactions
- **Rows:** ~3,500 (+ some duplicates)  
- **Fields:** transaction_id, transaction_ts, store_id, customer_id, product_id, quantity, unit_price, discount_pct, payment_method, channel, review_rating, promo_meta_json, pre_tax_amount, tax_rate, total_amount, is_returned  
- Simulates sales transactions across all stores and channels.  
- Includes **JSON column (`promo_meta_json`)** with campaign details, **missing values**, **duplicates**, and a few **outliers** for data cleaning practice.

### 5. Returns
- **Rows:** Subset of transactions  
- **Fields:** transaction_id, product_id, customer_id, store_id, transaction_ts, quantity, total_amount, return_ts, reason  
- Captures returned products and reasons.

---

## Learning Objectives

By working with this dataset in **Databricks (PySpark)**, students will:

1. **Load and inspect data** from CSV or Excel files.
2. **Clean data:** handle missing values, duplicates, and outliers.
3. **Transform data:** cast types, parse JSON, derive metrics, and join dimension tables.
4. **Aggregate and analyse:** compute KPIs (monthly revenue, top products, loyalty tier impact).
5. **Apply window functions** and **UDFs** for advanced analytics.
