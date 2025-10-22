# Sales Insights Data Analysis Project

## Project Overview

This project involves building a Power BI dashboard for AtliQ Hardware, a hardware distribution company based in Delhi, India. The dashboard provides sales directors with real-time insights into sales performance, market trends, customer behavior, and product analysis.
<img width="569" height="344" alt="image" src="https://github.com/user-attachments/assets/cfa533d5-855b-4ba3-84aa-a58c3fa677db" />


### The Problem
- Sales directors couldn't track sales performance easily.
- Data was scattered across regions.
- Manual Excel reports took hours.
- No real-time insights available.

### The Solution
- Built an interactive Power BI dashboard showing total revenue, sales quantity, market performance, customer insights, and revenue trends.
- Centralized data access and enabled data-driven decisions.

## Database Structure

**Database:** sales (MySQL 8.0)

**5 Tables:**

1. **transactions** (Fact Table) - ~150,000 records
   - product_code: Which product sold
   - customer_code: Which customer bought
   - market_code: Which city/market
   - order_date: When (2017-2020)
   - sales_qty: How many units
   - sales_amount: Revenue
   - currency: INR or USD

2. **customers** (38 customers)
   - customer_code: Unique ID (Cus001, Cus002...)
   - customer_name: Customer name
   - customer_type: "Brick & Mortar" or "E-Commerce"

3. **markets** (17 markets)
   - markets_code: Unique ID (Mark001, Mark002...)
   - markets_name: City (Delhi NCR, Mumbai, etc.)
   - zone: Region (North, South, Central)

4. **products** (279 products)
   - product_code: Unique ID (Prod001, Prod002...)
   - product_type: "Own Brand" or "Distribution"

5. **date** (1,126 dates)
   - date: Calendar date
   - year: 2017, 2018, 2019, 2020
   - month_name: January, February, etc.

## Data Cleaning (Power Query)

**Problems Fixed:**

- **Currency Issue:** Mixed INR and USD transactions. Converted all USD to INR (1 USD = 83 INR).
- **Invalid Data:** Removed sales with -1 or 0 amounts. Filtered out New York & Paris transactions (kept India only).
- **Text Cleanup:** Removed '\r' characters from product types and dates. Trimmed extra spaces.
- **Data Types:** Set correct types: dates as Date, amounts as Number, etc.

## Data Model (Star Schema)

```
customers ──→ transactions ←── markets
              ↓
            date
              ↑
          products
```

**Relationships:**
- All tables connect to `transactions` (fact table).
- One-to-Many relationships.
- Enables filtering across all visuals.

## Dashboard Features

### 5 Main Visualizations:

1. **Revenue by Markets** (Bar Chart)
   - Shows which cities perform best.
   - Delhi NCR dominates with 53%.

2. **Sales Qty by Markets** (Bar Chart)
   - Shows units sold by city.
   - Delhi NCR leads with 1M units.

3. **Revenue Trend** (Line Chart)
   - Shows revenue over time.
   - Reveals declining trend in 2019-2020.

4. **Top 5 Customers** (Bar Chart)
   - Identifies most valuable customers.
   - Electricalsara Stores is #1.

5. **Top 5 Products** (Bar Chart)
   - Shows best-selling products.

### Interactive Filters:
- Year (2017, 2018, 2019, 2020)
- Month (Dynamic selection)

## Key Metrics and Insights

### Top KPIs:
- **Total Revenue:** ₹984.87M
- **Sales Quantity:** 2M units
- **Time Period:** June 2017 - June 2020

### Top Markets:
1. Delhi NCR - ₹519.57M (53%)
2. Mumbai - ₹150.08M
3. Ahmedabad - ₹132.31M

### Top Customers:
1. Electricalsara Stores - ₹413.33M
2. Electricalslytical - ₹49.64M
3. Excel Stores - ₹49.12M

### Top Products:
1. Prod243 - ₹4.67M
2. Prod047 - ₹4.45M
3. Prod165 - ₹4.22M

### Key Insights:
- **Market Concentration Risk:** Delhi NCR = 53% of total revenue. Top 3 markets = 81% of revenue.
- **Customer Concentration Risk:** Single customer (Electricalsara) = 42% of revenue. Top 5 customers = 61% of revenue.
- **Declining Performance:** Revenue dropped sharply in 2019-2020 (January 2020: ~₹15M).
- **Balanced Product Portfolio:** No single product dominates.

## Recommendations

### Immediate Actions:
1. Investigate 2019-2020 decline - Find root cause.
2. Reduce customer dependency - Acquire more customers.
3. Expand to underperforming markets - Grow sales in smaller cities.

### Strategic Actions:
1. Develop retention strategy for top customers.
2. Create market-specific sales plans.
3. Focus on underperforming zones (South, Central).

## Technical Summary

### Tools Used:
- **Database:** MySQL 8.0
- **ETL:** Power Query
- **Visualization:** Power BI Desktop
- **Data Model:** Star Schema

### Data Flow:
```
MySQL DB → Power BI → Clean Data → Create Model → Build Dashboard
```

**Key Transformations:**
- Currency normalization (USD → INR)
- Invalid data removal
- Text cleaning
- Data type corrections

## Project Deliverables
- ✅ Power BI Dashboard (.pbix file) - Interactive visuals, real-time filtering, drill-down capabilities.
- ✅ SQL Database Dump - Complete schema, sample data, 150K+ transactions.
- ✅ Documentation - Technical specs, data dictionary, user guide.

### Instructions to Setup MySQL on Your Local Computer

 1.SQL database dump is in `db_dump.sql` file. Download `db_dump.sql` file to your local computer and import it as per instructions given in the tutorial video.

### Data Analysis Using SQL

1. Show all customer records
   ```sql
   SELECT * FROM customers;
   ```

2. Show total number of customers
   ```sql
   SELECT count(*) FROM customers;
   ```

3. Show transactions for Chennai market (market code for Chennai is Mark001)
   ```sql
   SELECT * FROM transactions WHERE market_code='Mark001';
   ```

4. Show distinct product codes that were sold in Chennai
   ```sql
   SELECT DISTINCT product_code FROM transactions WHERE market_code='Mark001';
   ```

5. Show transactions where currency is US dollars
   ```sql
   SELECT * FROM transactions WHERE currency="USD";
   ```

6. Show transactions in 2020 joined by date table
   ```sql
   SELECT transactions.*, date.* FROM transactions INNER JOIN date ON transactions.order_date=date.date WHERE date.year=2020;
   ```

7. Show total revenue in year 2020
   ```sql
   SELECT SUM(transactions.sales_amount) FROM transactions INNER JOIN date ON transactions.order_date=date.date WHERE date.year=2020 AND (transactions.currency="INR\r" OR transactions.currency="USD\r");
   ```

8. Show total revenue in year 2020, January Month
   ```sql
   SELECT SUM(transactions.sales_amount) FROM transactions INNER JOIN date ON transactions.order_date=date.date WHERE date.year=2020 AND date.month_name="January" AND (transactions.currency="INR\r" OR transactions.currency="USD\r");
   ```

9. Show total revenue in year 2020 in Chennai
   ```sql
   SELECT SUM(transactions.sales_amount) FROM transactions INNER JOIN date ON transactions.order_date=date.date WHERE date.year=2020 AND transactions.market_code="Mark001";
   ```

### Data Analysis Using Power BI

1. Formula to create norm_amount column
   ```
   = Table.AddColumn(#"Filtered Rows", "norm_amount", each if [currency] = "USD" or [currency] = "USD#(cr)" then [sales_amount]*75 else [sales_amount], type any)
   ```

### Quick Reference: DAX Measures

- **Total Revenue** = `SUM(transactions[sales_amount])`
- **Total Sales Qty** = `SUM(transactions[sales_qty])`
- **YoY Growth %** = `DIVIDE([Total Revenue] - [Revenue LY], [Revenue LY], 0)`



