# Data Cleaning with Excel Power Query: Online Retail Dataset

## 📖 Project Overview
This section outlines the data cleaning phase of an analytical project using the Online Retail dataset from the TATA Data Visualization Job Simulation program. 

Dataset - [Google Drive link](https://docs.google.com/spreadsheets/d/1ahoOgRCregEMb_E34YU1XSuOib3XK0as/edit?usp=sharing&ouid=105755353470221483980&rtpof=true&sd=true)

Raw datasets rarely come ready for analysis. The primary objective of this phase was to construct an automated, streamlined data transformation pipeline using Excel Power Query. This approach eliminates manual, repetitive cleaning efforts, ensures consistency, and significantly improves overall workflow efficiency.

---

## 🔍 Dataset Characteristics
Before beginning the transformation process, the dataset contained multiple structural and quality issues that required remediation:
* Duplicate rows across multiple dimensions.
* Null values in the `Description` (Product Name) and `CustomerID` columns.
* Anomalous negative values in the `Quantity` column.
* Invalid zero values in the `UnitPrice` and `Quantity` columns.
* Inconsistent categorical naming conventions (e.g., USA vs. United States).

**Dimensionality Transformation:**
* **Before Cleaning:** 541,910 rows x 8 columns
* **After Cleaning:** 534,132 rows x 12 columns

---

## 🛠 Step-by-Step Data Cleaning Workflow

### 1. Pre-Processing 
* Identified and removed 5,268 duplicate rows (out of 536,641 total rows) directly in Excel, ensuring all columns, including date and time stamps, were evaluated.
* Loaded the refined dataset into the Power Query Editor for automated transformations.

### 2. Text Transformation & Standardization
* Utilized the **Replace Values** function to populate blank cells in the `Description` column with the string "Unknown".
* Applied the **Trim** function to the `Description` column to strip trailing and leading whitespaces.
* Standardized geographic data by replacing "RSA" with "Republic of South Africa".
* Standardized geographic data by replacing "USA" with "United States".


### 3. Handling Anomalies & Feature Engineering
* Handled negative values in the `Quantity` column by introducing a new `TransactionType` conditional column to clearly separate sales from returns. 

**M Formula for TransactionType:**
```powerquery
if [Quantity] < 0 then "Return" else "Sale"
```
* Applied a Number Filter to strictly remove any rows containing a zero value in the Quantity or UnitPrice columns.
* Handled null values in the `CustomerID` column by introducing a new `CustomerType` to separate 'Registered' and 'Guest' customers.

**M Formula for CustomerType:**
```powerquery
if [CustomerID] = null then "Guest" else "Registered"
```

* Split the InvoiceDate column into two distinct columns (Date and Time) to establish a consistent chronological format for time-series analysis.
* Added a new `Revenue`  column to calculate the total financial value of each row, setting the foundation for the final analysis.
  
**M Formula for Revenue:**
```powerquery
[Quantity] * [UnitPrice]
```

---
## Applied Steps Overview in Power Query

![Alt image](https://github.com/yanheinaung23-eng/E-commerce-Sales-and-Customers-RFM-Analysis-Full-Project/blob/a3ec3312f59dabcf598f8a1b03239a69e6003d5f/Data%20Cleaning%20in%20Power%20Query/Documents/Applied%20Steps%20in%20Power%20Query.png)
