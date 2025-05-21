# Inventory_Mangement_SQL

## Project Overview

This project demonstrates the design and management of an **Inventory Management System** using SQL. It includes schema creation, data loading, complex queries, stored procedures, triggers, and events — all essential for building a real-world database-driven application.

---

## Technologies Used

- **MySQL**
- SQL Stored Procedures & Triggers
- SQL Events
- CSV Data Import

---

## Database Schema

### Tables Created:
- `products`  
  - `product_id`, `product_name`, `category`, `price`, `stock`
- `orders`  
  - `order_id`, `customer_id`, `product_id`, `order_date`, `quantity`, `status`, `total_price`
- `customers`  
  - `customer_id`, `customer_name`, `email`, `join_date`, `region`
- `category_revenue`, `monthlyrevenuereport` for analytical reporting

---

## Data Sources

Data is loaded from:
- `products.csv`
- `orders.csv`
- `customers.csv`

Each table uses `LOAD DATA INFILE` to bulk insert CSV content into the database.

---

## SQL Queries and Analytics

- Top 5 customers by total spend
- Products never ordered
- Customers with average order value > 200
- Purchase trends by region post-2022
- Monthly revenue analytics for 2023
- Order cancellation analysis
- Lifetime customer value

---

## Stored Procedures

### Included Procedures:
- `get_customer_count` - Get total number of orders by customer ID
- `customer_data` - Dynamic lookup of customer-product spend
- `product_category` - Insert category-wise revenue into summary table
- `new_insert_order` - Insert new order with error handling
- `createorder` - Full order logic with stock deduction and transactions
- `getcustomerbydaterange` - Fetch customers ordering in a date range
- `generatemonthlyrevenuereport` - Generates region-wise revenue report

---

## Triggers & Events

- **Trigger:** `updatestockordernew`  
  Updates stock after new order and prevents negative inventory.

- **Event:** `monthlyrevenuetrigger`  
  Automatically generates revenue report on the last day of each month.

---

## Sample Procedure Call

```sql
CALL createorder (4, 104, '2024-12-24', 2, 15, @result_message);
SELECT @result_message;

CREATE EVENT monthlyrevenuetrigger
ON SCHEDULE EVERY 1 MONTH
STARTS LAST_DAY(CURDATE()) + INTERVAL 1 SECOND
DO CALL generatemonthlyrevenuereport();
