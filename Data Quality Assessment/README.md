![Alt image](https://github.com/yanheinaung23-eng/E-commerce-Sales-and-Customers-RFM-Analysis-Full-Project/blob/85366179158a1ab43e68f30a72c7f45f463b232d/Data%20Quality%20Assessment/Documents/Data%20Quality%20Assessment.png)
### E-commerce Sales & Customers RFM Analysis Project — Data Quality Assessment
 
> A systematic, SQL-driven data quality audit performed on the UCI Online Retail dataset before any analytical or modelling work begins.
 
---
 
## 📋 Table of Contents
 
- [Overview](#overview)
- [Tech Stack](#tech-stack)
- [Dataset at a Glance](#dataset-at-a-glance)
- [Step-by-Step Assessment](#step-by-step-assessment)
  - [Step 1 — Schema & Data Types](#step-1--schema--data-types)
  - [Step 2 — Row Count](#step-2--row-count)
  - [Step 3 — InvoiceNo Integrity](#step-3--invoiceno-integrity)
  - [Step 4 — StockCode Integrity](#step-4--stockcode-integrity)
  - [Step 5 — Description Cardinality](#step-5--description-cardinality)
  - [Step 6 — Quantity Distribution & Outliers](#step-6--quantity-distribution--outliers)
  - [Step 7 — Cancelled Orders](#step-7--cancelled-orders)
  - [Step 8 — Date Range Validation](#step-8--date-range-validation)
  - [Step 9 — UnitPrice Integrity](#step-9--unitprice-integrity)
  - [Step 10 — Revenue Integrity](#step-10--revenue-integrity)
  - [Step 11 — CustomerID Integrity](#step-11--customerid-integrity)
  - [Step 12 — Categorical Columns](#step-12--categorical-columns)
- [Summary](#summary)
---
 
## Overview
 
Before diving into EDA or answering Stakeholders questions, a thorough **data quality assessment** was conducted entirely in **SQL (SQLite via ipython-sql)**. The goal was to:
 
- Confirm schema correctness and expected data types
- Detect anomalies, outliers, and edge cases in every column
- Understand the business meaning behind irregular values (e.g. cancellations)
- Establish clean filters for downstream analysis
---
 
## Tech Stack
 
| Tool | Purpose |
|---|---|
| Python / Pandas | Data loading, distribution summaries |
| SQLite | In-session relational queries |
| ipython-sql (`%%sql`) | SQL magic cells inside Jupyter/Colab |
| Google Colab | Execution environment |
 
---
 
## Dataset at a Glance
 
The dataset is the **UCI Online Retail** dataset (cleaned version), loaded into a SQLite table called `online_sales`.
 
| Column | Type | Description |
|---|---|---|
| `InvoiceNo` | TEXT | 6-digit invoice number; prefix `C` = cancellation |
| `StockCode` | TEXT | 5-character product code |
| `Description` | TEXT | Product name |
| `Quantity` | INTEGER | Units per transaction (negative = cancelled) |
| `InvoiceDate` | DATETIME | Date & time of transaction |
| `UnitPrice` | REAL | Price per unit (GBP) |
| `Revenue` | REAL | Derived: Quantity × UnitPrice |
| `CustomerID` | REAL | 5-digit customer identifier |
| `Country` | TEXT | Customer's country |
| `TransactionType` | TEXT | Categorical transaction label |
| `CustomerType` | TEXT | Categorical customer label |
 
---
 
## Step-by-Step Assessment
 
### Step 1 — Schema & Data Types
 
**Goal:** Confirm every column has the expected data type before running any calculations.
 
```sql
PRAGMA table_info(online_sales);
```
 
**Finding** 
`CustomerID` column is `REAL` and it should be `INTEGER` or `TEXT`.
 
---
 
### Step 2 — Row Count
 
**Goal:** Establish the total number of transactions as a baseline for all subsequent checks.
 
```sql
SELECT COUNT(*)
FROM online_sales;
```
**Finding:** Total transactions 534131
 
---
 
### Step 3 — InvoiceNo Integrity
 
**Goal:** Detect non-standard invoice numbers that may represent special transaction types.
 
```sql
-- Length distribution
SELECT
  MAX(LENGTH(InvoiceNo)) AS max_length,
  MIN(LENGTH(InvoiceNo)) AS min_length,
  ROUND(AVG(LENGTH(InvoiceNo)), 2) AS avg_length
FROM online_sales;
```
**Finding:** max_length: 7 | min_length: 6 | avg_length: 6.02

```sql
-- Inspect records with length > 6
SELECT *
FROM online_sales
WHERE LENGTH(InvoiceNo) > 6
LIMIT 10;
```
 
**Finding:** Invoice numbers with length > 6 have a `C` prefix. These are **cancellation records** — not data errors.
 
---
 
### Step 4 — StockCode Integrity
 
**Goal:** Verify that product codes follow the expected 5-character format; flag anomalies.
 
```sql
-- Length distribution
SELECT
  MAX(LENGTH(StockCode)) AS max_length,
  MIN(LENGTH(StockCode)) AS min_length,
  ROUND(AVG(LENGTH(StockCode)), 2) AS avg_length
FROM online_sales;
```
**Finding:** max_length: 12 | min_length: 1 | avg_length: 5.09

```sql
-- Inspect non-standard codes
SELECT *
FROM online_sales
WHERE LENGTH(StockCode) > 5 OR LENGTH(StockCode) < 5
LIMIT 10;
```
**Finding:** Some StockCode are having more than 5 in length but very few. Detected `POST` in StockCode.

```sql
-- Total distinct product codes
SELECT COUNT(*)
FROM (
  SELECT DISTINCT StockCode
  FROM online_sales
);
```
**Finding:** Total distinct stock codes: 3938

---
 
### Step 5 — Description Cardinality
 
**Goal:** Compare the number of distinct descriptions against distinct StockCodes to detect mapping inconsistencies.
 
```sql
SELECT COUNT(*)
FROM (
  SELECT DISTINCT Description
  FROM online_sales
);
```
 
**Findings:** Total distinct description: 4031 and `COUNT(DISTINCT Description) > COUNT(DISTINCT StockCode)`, the same product code maps to multiple description strings.
 
---
 
### Step 6 — Quantity Distribution & Outliers
 
**Goal:** Understand the spread of order quantities and identify extreme values that could skew aggregations.
 
```python
# Statistical summary with key percentiles
df['Quantity'].describe(percentiles=[0.25, 0.5, 0.75, 0.9, 0.95, 0.99])
```
![Alt image](https://github.com/yanheinaung23-eng/E-commerce-Sales-and-Customers-RFM-Analysis-Full-Project/blob/eb6273ed76010e1afd87b6ea72afaa236a331a9a/Data%20Quality%20Assessment/Documents/Quantity%20Distribution%20Percentile.png)

**Finding:** 
* 25%: A quarter of the transactions are for a quantity of 1 or less.

* 75%: 75% of all transactions are for 10 items or fewer.

* 99%: 99% of the transactions involve quantities of 100 or less.

> ⚠️The data is heavily skewed to the right. The average is being artificially dragged up by a small number of massive wholesale-style orders (like the 80,995 anomaly).

```sql
-- Max, min, and average
SELECT
  MAX(Quantity) AS max_quantity,
  MIN(Quantity) AS min_quantity,
  AVG(Quantity)  AS avg_quantity
FROM online_sales;
```
 
```sql
-- Inspect extreme values
SELECT *
FROM online_sales
WHERE Quantity = 80995 OR Quantity = -80995;
```
 
```sql
-- Total positive quantity sold
SELECT SUM(Quantity) AS total_quantity
FROM online_sales
WHERE Quantity > 0;
```
 
**Finding:** The extreme values of ±80,995 belong to a **single customer** who ordered and then cancelled 80,995 units of paper craft within 12 minutes.
 
---
 
### Step 7 — Cancelled Orders
 
**Goal:** Quantify cancellations and check transaction type.
 
```sql
SELECT COUNT(*)
FROM online_sales
WHERE Quantity < 0;
```
**Finding:** Total cancelled orders: 9251

```sql
-- Confirm transaction type categories
SELECT DISTINCT TransactionType
FROM online_sales;
```
**Finding:** Only two type - Sale | Return

---
 
### Step 8 — Date Range Validation
 
**Goal:** Confirm the full temporal coverage of the dataset and compute the number of months available for trend analysis.
 
```sql
SELECT
  MIN(InvoiceDate) AS start_date,
  MAX(InvoiceDate) AS end_date,
  (
    (CAST(strftime('%Y', MAX(InvoiceDate)) AS INTEGER) -
     CAST(strftime('%Y', MIN(InvoiceDate)) AS INTEGER)) * 12
    +
    (CAST(strftime('%m', MAX(InvoiceDate)) AS INTEGER) -
     CAST(strftime('%m', MIN(InvoiceDate)) AS INTEGER))
  ) AS order_range_months
FROM online_sales;
```
**Finding:** Start date from 2010-12-01 to end date 2011-12-09. 
 
---
 
### Step 9 — UnitPrice Integrity
 
**Goal:** Detect pricing anomalies including zero-price, negative-price, and extreme-price records.
 
```sql
SELECT
  MAX(UnitPrice) AS max_price,
  MIN(UnitPrice) AS min_price,
  AVG(UnitPrice) AS avg_price
FROM online_sales;
```
**Finding:** max_price: 38970 | min_price: -11062.06 | avg_price: 4.6544

```python
df['UnitPrice'].describe(percentiles=[0.25, 0.5, 0.75, 0.9, 0.95, 0.99])
```
![Alt image](https://github.com/yanheinaung23-eng/E-commerce-Sales-and-Customers-RFM-Analysis-Full-Project/blob/843fa9ce13682583796a61a6c5aee226d934e048/Data%20Quality%20Assessment/Documents/UnitPrice%20Distribution%20Percentile.png)

**Finding:**
* 50% (Median): Half of all items sold cost 2.10 or less.

* 75%: Three-quarters of all items cost 4.13 or less.

* 99%: 99% of all items in this dataset are priced at 18.00 or less.

it paints a picture of a retailer selling cheap, everyday items.


```sql
-- Inspect the highest unit price
SELECT *
FROM online_sales
WHERE UnitPrice = 38970;
```
**Finding:** There is only one line and the stock code is 'M'. It could be a 'manual' entry to keep a note for the specific customer ID.

```sql
-- Inspect negative unit prices
SELECT *
FROM online_sales
WHERE UnitPrice < 0
LIMIT 10;
```
 
**Finding:** Two records with negative `UnitPrice` represent **bad debt adjustments** posted on the same timestamp (`2011-08-12`). These are accounting entries, not product sales. Records with `UnitPrice = 0` also exist and are excluded from revenue calculations.
 
---
 
### Step 10 — Revenue Integrity
 
**Goal:** Validate the derived `Revenue` column (Quantity × UnitPrice) and establish true positive-revenue totals.
 
```sql
SELECT
  MIN(Revenue)          AS min_revenue,
  MAX(Revenue)          AS max_revenue,
  ROUND(SUM(Revenue), 2) AS total_sales
FROM online_sales
WHERE Revenue > 0;
```
**Finding:** min_revenue: 0.06 | max_revenue: 168469.6 | total_sales: 10642110.8 
 
```python
df['Revenue'].describe(percentiles=[0.25, 0.5, 0.75, 0.9, 0.95, 0.99])
```
![Alt image](https://github.com/yanheinaung23-eng/E-commerce-Sales-and-Customers-RFM-Analysis-Full-Project/blob/843fa9ce13682583796a61a6c5aee226d934e048/Data%20Quality%20Assessment/Documents/Revenue%20Distribution%20Percentile.png)

**Finding:**
* 50% (Median): Half of all transactions generate 9.90 or less in revenue.

* 75%: Three-quarters of all transactions generate 17.57 or less.

* 99%: 99% of all transactions bring in 182.60 or less.

This completely aligns with the previous findings of low quantities and low unit prices. The core business relies on a high volume of small, inexpensive purchases rather than big-ticket sales.
 
---
 
### Step 11 — CustomerID Integrity
 
**Goal:** Verify that all CustomerIDs conform to the expected 5-digit format.
 
```sql
SELECT
  MIN(LENGTH(CAST(CustomerID AS VARCHAR))) AS min_length_customerID,
  MAX(LENGTH(CAST(CustomerID AS VARCHAR))) AS max_length_customerID
FROM online_sales;
```
**Finding:** min_length_customerID: 7 | max_length_customerID: 7

```sql
-- Explore customer type classification
SELECT DISTINCT CustomerType
FROM online_sales;
```
**Finding:** There is only two types - Registered | Guest.
  
---
 
### Step 12 — Categorical Columns
 
**Goal:** Enumerate all distinct values in categorical columns to catch unexpected entries.
 
```sql
-- Countries represented in the dataset
SELECT DISTINCT Country
FROM online_sales;
```
 
**Finding:** There are 38 countries. Detected unexpected country entries (e.g. `"Unspecified"`, `"European Community"`).
 
---
 
## Summary
 
### Key Findings

- The `CustomerID` column is stored as **REAL** instead of a more appropriate **INTEGER** or **TEXT** data type.
- Invoice numbers prefixed with **"C"** represent legitimate cancellation transactions rather than data entry errors.
- Most `StockCode` values follow the expected 5-character format, but a small number of non-standard codes (e.g., `POST`, `M`) were identified.
- Product descriptions are not mapped one-to-one with StockCodes, resulting in more unique descriptions than unique products.
- Quantity, UnitPrice, and Revenue distributions are heavily right-skewed, with a small number of extreme transactions significantly influencing averages.
- An extreme outlier was identified where a customer purchased and later cancelled **80,995 units** of a single product.
- The dataset contains **9,251 return/cancellation transactions**, represented by negative quantities.
- Negative unit prices were found and correspond to accounting adjustments (bad debt write-offs) rather than product sales.
- Several zero-price transactions exist and should be reviewed before revenue analysis.
- Customer records consist of both **Registered** and **Guest** customer types.
- The dataset includes transactions from **38 countries**, including special categories such as `"Unspecified"` and `"European Community"`.
 
---
 
*This is the part of the E-commerce Sales & Customers RFM Analysis project. Next step is to conduct EDA.*
