# Digital-Transaction-Analysis-SQL-Project
## Project Overview

**Project Title**: Digital Transaction SQL Analysis

In this project, I will be analyzing digital transactions from a dataset derived from Kaggle. This project is designed to demonstrate my SQL knowledge and proficiency by performing a set of queries

## Objectives

1. **Import CSV file to MySQL Workbench**
2. **Data Cleaning**: Check if there are any missing or null values.
3. **Exploratory Data Analysis**: Perform exploratory data analysis using SQL to understand the dataset.
4. **Key Findings**: Find significant conclusions that can be drawn from the explored data.

## Project Structure

### 1. Data Cleaning
```sql
-- Check for missing or NULL values
SELECT
    COUNT(*) - COUNT(transaction_id) AS missing_transaction_id,
    COUNT(*) - COUNT(user_id) AS missing_user_id,
    COUNT(*) - COUNT(transaction_date) AS missing_transaction_date,
    COUNT(*) - COUNT(product_category) AS missing_product_category,
    COUNT(*) - COUNT(product_name) AS missing_product_name,
    COUNT(*) - COUNT(merchant_name) AS missing_merchant_name,
    COUNT(*) - COUNT(product_amount) AS missing_product_amount,
    COUNT(*) - COUNT(transaction_fee) AS missing_transaction_fee,
    COUNT(*) - COUNT(cashback) AS missing_cashback,
    COUNT(*) - COUNT(loyalty_points) AS missing_loyalty_points,
    COUNT(*) - COUNT(payment_method) AS missing_payment_method,
    COUNT(*) - COUNT(transaction_status) AS missing_transaction_status,
    COUNT(*) - COUNT(merchant_id) AS missing_merchant_id,
    COUNT(*) - COUNT(device_type) AS missing_device_type,
    COUNT(*) - COUNT(location) AS missing_location
FROM digital_wallet_transactions;
```

```sql
-- Delete any NULL or missing values
DELETE FROM digital_wallet_transactions
WHERE
    transaction_id IS NULL OR
    user_id IS NULL OR
    transaction_date IS NULL OR
    product_category IS NULL OR
    product_name IS NULL OR
    merchant_name IS NULL OR
    product_amount IS NULL OR
    transaction_fee IS NULL OR
    cashback IS NULL OR
    loyalty_points IS NULL OR
    payment_method IS NULL OR
    transaction_status IS NULL OR
    merchant_id IS NULL OR
    device_type IS NULL OR
    location IS NULL;
```

### 2. Data Exploration
- **Record Count**: Determine the total number of records in the dataset.
```sql
SELECT COUNT(*) AS total_records
FROM digital_wallet_transactions;
-- OUTPUT: 5000
```
- **Customer Count**: Find out how many unique customers are in the dataset.
```sql
SELECT COUNT(DISTINCT user_id) AS unique_customers
FROM digital_wallet_transactions;
-- OUTPUT: 3932
```
- **Category Count**: Identify all unique product categories in the dataset.
```sql
SELECT DISTINCT product_category
FROM digital_wallet_transactions;
```

### 3. Data Analysis & Findings

The following SQL queries were developed to answer specific transaction questions:

```sql
-- Q1. Find the total amount of transactions
SELECT COUNT(*) AS total_transactions
FROM digital_wallet_transactions;
```

```sql
-- Q2. Find the location with the most users
SELECT
    location,
    COUNT(DISTINCT user_id) AS user_count
FROM
    digital_wallet_transactions
GROUP BY
    location
ORDER BY
    user_count DESC
LIMIT 1;


```
```sql
-- Q3. Find the product category with the most failed transactions
SELECT
    product_category,
    COUNT(*) AS failed_transactions_count
FROM
    digital_wallet_transactions
WHERE
    transaction_status = 'Failed'
GROUP BY
    product_category
ORDER BY
    failed_transactions_count DESC
LIMIT 1;
```
```sql
-- Q4. Find the average product cost in descending order
SELECT
    product_category,
    AVG(product_amount) AS avg_product_amount
FROM
    digital_wallet_transactions
GROUP BY
    product_category
ORDER BY
    avg_product_amount DESC;
```
```sql
-- Q5. Find the payment method that returns the average cashback in descending order
SELECT
    payment_method,
    AVG(cashback) AS total_cashback
FROM
    digital_wallet_transactions
GROUP BY
    payment_method
ORDER BY
    total_cashback DESC;
```
```sql
-- Q6. Display the row for the highest product amount of a successfull transaction
SELECT 
    *
FROM
    digital_wallet_transactions
WHERE
    transaction_status = 'Successful'
ORDER BY product_amount DESC
LIMIT 1;
```
```sql
-- Q7. Find the most frequent users in descending order
SELECT
    user_id,
    COUNT(*) AS transaction_count
FROM
    digital_wallet_transactions
GROUP BY
    user_id
ORDER BY
    transaction_count DESC;
```
```sql
-- Q8. Find the 10 users with the most unsuccessfull transactions
SELECT
    user_id,
    COUNT(*) AS failed_transaction_count
FROM
    digital_wallet_transactions
WHERE
    transaction_status = 'Failed'
GROUP BY
    user_id
ORDER BY
    failed_transaction_count DESC
LIMIT 10;
```
```sql
-- Q9. Find the top 5 merchants with the most money received
SELECT
    merchant_name,
    SUM(product_amount) AS total_received
FROM
    digital_wallet_transactions
GROUP BY
    merchant_name
ORDER BY
    total_received DESC
LIMIT 5;
```
```sql
-- Q10. Find the best selling month in each year
WITH monthly_sales AS (
    SELECT
        EXTRACT(YEAR FROM transaction_date) AS year,
        EXTRACT(MONTH FROM transaction_date) AS month,
        SUM(product_amount) AS total_sales
    FROM
        digital_wallet_transactions
    GROUP BY
        year, month
)
SELECT
    year,
    month,
    total_sales
FROM
    monthly_sales
WHERE
    (year, total_sales) IN (
        SELECT
            year,
            MAX(total_sales) AS max_sales
        FROM
            monthly_sales
        GROUP BY
            year
    )
ORDER BY
    year DESC, month DESC;
```

## Findings

- **Customer Demographics**: A deep dive into user demographics, identifying regions with the highest concentration of customers.
- **Purchase Trends**: The analysis indicates a higher volume of purchases in the second half of the year compared to the first, suggesting a possible shift in consumer buying behavior.
- **Customer Insights**: The product category with the highest number of failed transactions is "Gaming Credits." This trend may suggest that children, could be attempting to make unauthorized purchases using their parents credit cards.

## Conclusion
This analysis serves as a comprehensive approach to utilizing SQL for business insights, covering data extraction, pattern analysis and performance evaluation. The findings from these SQL queries can help drive business decisions by uncovering transaction trends, sales performance, and customer demographics.

