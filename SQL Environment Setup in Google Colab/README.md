![Alt image](https://github.com/yanheinaung23-eng/E-commerce-Sales-and-Customers-RFM-Analysis-Full-Project/blob/cf97e4004578ca0bae8bfc8079f8edd946b56811/Data%20Cleaning%20in%20Power%20Query/Documents/Google%20Collab%20x%20SQLite.png)
 
> This is my step by step to how to setup the SQL Environment in Google Colab as part of E-commerce Sales and Customers RFM Analysis Full Project.
 
---
 
## 📋 Prerequisites
 
| Requirement | Version |
|---|---|
| Python | 3.8+ |
| Google Colab | any |
| Dataset | `Online Retail Cleaned.csv` |
 
> **Note:** The setup below is designed for **Google Colab**. The dataset upload step uses Colab's built-in file picker. If we are running this locally in Jupyter, replace the upload step with a direct file path instead.
 
---
 
## ⚙️ SQL Environment Setup
 

### Step 1 — Install Compatible Packages
 
The default Colab environment ships with versions of `ipython-sql` and `prettytable` that conflict with each other. Remove them first, then install the pinned compatible versions.
 
```python
# Remove potentially incompatible versions
!pip uninstall -y ipython-sql prettytable
 
# Install compatible versions
!pip install ipython-sql==0.5.0
!pip install prettytable==3.10.0 openpyxl
```
 
> ⚠️ **Why pin these versions?** `ipython-sql 0.5.0` introduced a breaking change in how it formats results. Mismatched `prettytable` versions will raise a `TypeError` at query time. Always use the exact versions above.
 
---
 
### Step 2 — Import the Libraries
 
```python
import sql
import prettytable
```
 
---
 
### Step 3 — Load the SQL Magic Extension
 
This activates the `%sql` and `%%sql` cell magic commands used throughout the notebook.
 
```python
%load_ext sql
```
 
---
 
### Step 4 — Upload the Dataset
 
Ran this cell to open Colab's file picker and upload your CSV file. Select `Online Retail Cleaned.csv` when prompted.
 
```python
from google.colab import files
 
uploaded = files.upload()
```
 
> 📁 Clicked upload and the file was saved to `/content/` inside the Colab runtime. The next step references it by the exact filename `Online Retail Cleaned.csv`.
 
---
 
### Step 5 — Load the Data into SQLite
 
This step reads the CSV into a pandas DataFrame and writes it into a local SQLite database (`my_data.db`), creating a table called `online_sales`.
 
```python
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
import sqlite3
 
# Read CSV — latin1 encoding is required to handle special characters
df = pd.read_csv("/content/Online Retail Cleaned.csv", encoding='latin1')
 
# Parse the date column
df["InvoiceDate"] = pd.to_datetime(df["InvoiceDate"])
 
# Create a local SQLite database
conn = sqlite3.connect("my_data.db")
 
# Write the DataFrame to a SQL table
df.to_sql(
    "online_sales",
    conn,
    index=False,
    if_exists="replace"
)
 
print("Data uploaded successfully!")
print("Shape:", df.shape)
```
 
**Expected output:**
```
Data uploaded successfully!
Shape: (XXXXXX, N)
```
 
> 💡 `if_exists="replace"` means re-running this cell will drop and recreate the table. Safe to re-run if you need a fresh start.
 
---
 
### Step 6 — Connect SQL Magic to the Database
 
Pointed the `%sql` magic at the SQLite file created in the previous step.
 
```python
%sql sqlite:///my_data.db
```
 
---
 
### Step 7 — Verify the Setup
 
Ran these two test queries to confirm everything is working correctly.
 
**Check that the table exists:**
```sql
%%sql
 
SELECT name
FROM sqlite_master
WHERE type='table';
```
 
**Preview the first 10 rows:**
```sql
%%sql
 
SELECT *
FROM online_sales
LIMIT 10;
```
   
---
 
## 📊 What This Notebook Covers
 
Once the environment is set up, the notebook walks through:
 
- **Data Quality Assessment** — schema inspection, outlier detection, null checks
- **EDA** — sales trends, top products, revenue by country and customer type
- **Advanced Analysis**
  - Q1: Seasonality — UK vs. International markets
  - Q2: Revenue concentration risk & expansion opportunities (Pareto analysis)
  - Q3: RFM segmentation to identify high-value customers
  - Q4: Customer retention cohort analysis
---
 
## 🛠️ Troubleshooting
 
| Error | Likely Cause | Fix |
|---|---|---|
| `TypeError` on first `%%sql` cell | `prettytable` version mismatch | Re-run Step 1 and restart the runtime |
| `UnicodeDecodeError` on CSV read | Wrong encoding | Ensure `encoding='latin1'` is set in `pd.read_csv` |
| `OperationalError: no such table` | DB not yet created | Run Steps 4–6 before any SQL query cells |
| File picker does nothing | Running outside Colab | Replace Step 4 with `pd.read_csv("path/to/your/file.csv", ...)` |
 
---
 
