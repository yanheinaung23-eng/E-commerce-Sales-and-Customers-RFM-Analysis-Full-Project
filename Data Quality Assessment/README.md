# 🔍 Data Quality Assessment
### E-commerce Sales & Customers — RFM Analysis Project
 
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
- [Findings Summary](#findings-summary)
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
| `CustomerID` | INTEGER | 5-digit customer identifier |
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
**Finding**
max_length: 7 | min_length: 6 | avg_length: 6.02

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
 
```sql
-- Inspect non-standard codes
SELECT *
FROM online_sales
WHERE LENGTH(StockCode) > 5 OR LENGTH(StockCode) < 5
LIMIT 10;
```
 
```sql
-- Total distinct product codes
SELECT COUNT(*)
FROM (
  SELECT DISTINCT StockCode
  FROM online_sales
);
```
 

 
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
 
**Finding:** `COUNT(DISTINCT Description) > COUNT(DISTINCT StockCode)`, the same product code maps to multiple description strings — often due to typos or capitalisation differences in the source data.
 
---
 
### Step 6 — Quantity Distribution & Outliers
 
**Goal:** Understand the spread of order quantities and identify extreme values that could skew aggregations.
 
```python
# Statistical summary with key percentiles
df['Quantity'].describe(percentiles=[0.25, 0.5, 0.75, 0.9, 0.95, 0.99])
```
 
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
 
**Goal:** Quantify the volume of cancellations and check transaction type.
 
```sql
SELECT COUNT(*)
FROM online_sales
WHERE Quantity < 0;
```
 
```sql
-- Confirm transaction type categories
SELECT DISTINCT TransactionType
FROM online_sales;
```
 
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
 
```python
df['UnitPrice'].describe(percentiles=[0.25, 0.5, 0.75, 0.9, 0.95, 0.99])
```
 
```sql
-- Inspect the highest unit price
SELECT *
FROM online_sales
WHERE UnitPrice = 38970;
```
 
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
 
```python
df['Revenue'].describe(percentiles=[0.25, 0.5, 0.75, 0.9, 0.95, 0.99])
```
 
**Why it matters:** Because `Revenue = Quantity × UnitPrice`, any negative value in either parent column produces a negative revenue row. The `WHERE Revenue > 0` filter is the standard clean baseline for all subsequent sales analysis.
 
---
 
### Step 11 — CustomerID Integrity
 
**Goal:** Verify that all CustomerIDs conform to the expected 5-digit format.
 
```sql
SELECT
  MIN(LENGTH(CAST(CustomerID AS VARCHAR))) AS min_length_customerID,
  MAX(LENGTH(CAST(CustomerID AS VARCHAR))) AS max_length_customerID
FROM online_sales;
```
 
```sql
-- Explore customer type classification
SELECT DISTINCT CustomerType
FROM online_sales;
```
 
**Why it matters:** In the original UCI dataset, a significant proportion of transactions have no `CustomerID` (guest checkouts). After cleaning, confirming all remaining IDs are 5 digits validates that the cleaning step worked correctly. This is critical for RFM analysis, which requires a stable customer identifier.
 
---
 
### Step 12 — Categorical Columns
 
**Goal:** Enumerate all distinct values in categorical columns to catch unexpected entries.
 
```sql
-- Countries represented in the dataset
SELECT DISTINCT Country
FROM online_sales;
```
 
**Why it matters:** Unexpected country entries (e.g. `"Unspecified"`, `"European Community"`) can affect geographic analysis. Enumerating them upfront allows informed decisions about inclusion or grouping.
 
---
 
## Findings Summary
 
| Column | Issue Found | Action Taken |
|---|---|---|
| `InvoiceNo` | Prefix `C` = cancellation (length > 6) | Filter with `InvoiceNo NOT LIKE 'C%'` for order counts |
| `StockCode` | Non-standard lengths (service/postage codes) | Noted; excluded from product-level analysis |
| `Description` | More distinct values than StockCodes | Acknowledged (typos/capitalisation) |
| `Quantity` | Extreme outlier ±80,995 (single event) | Retained; documented as legitimate |
| `Quantity` | Negative values = cancellations | Filter `Quantity > 0` for sales metrics |
| `UnitPrice` | Negative prices = bad debt adjustments | Filter `UnitPrice > 0` for revenue |
| `UnitPrice` | Zero prices exist | Excluded from revenue calculations |
| `Revenue` | Negative revenue from cancellations/adjustments | Filter `Revenue > 0` as clean baseline |
| `CustomerID` | All IDs confirmed as 5-digit post-cleaning | No further action needed |
| `Country` | `"Unspecified"` entries present | Noted for geographic analysis |
 
> **Clean data filter used throughout EDA:**
> ```sql
> WHERE Quantity > 0 AND UnitPrice > 0
> ```
 
---
 
*Part of the [E-commerce Sales & Customers RFM Analysis](../EDA_E_commerce_Sales_and_Customers_RFM_Analysis.ipynb) portfolio project.*
