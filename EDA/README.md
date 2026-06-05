
An Exploratory Data Analysis on a real-world online retail dataset using SQL (SQLite) and Python, uncovering patterns in sales performance, product demand, and customer behaviour.

---
 
## 🧰 Tools Used
 
- **SQLite + ipython-sql** — in-notebook SQL queries via `%%sql` magic
- **Pandas** — pulling SQL results into DataFrames
- **Matplotlib + Seaborn** — visualisations
---

## 📊 EDA Walkthrough
 
### Monthly Sales Trend
```python
sales_trend_df = pd.read_sql_query(
    """SELECT
    strftime('%Y-%m', InvoiceDate) AS year_month,
    ROUND(SUM(Revenue),2) AS total_revenue
    FROM online_sales
    WHERE Quantity > 0 AND UnitPrice > 0
    GROUP BY year_month
    ORDER BY year_month;""",
    conn
)

# Convert 'year_month' to datetime for proper plotting
sales_trend_df['year_month'] = pd.to_datetime(sales_trend_df['year_month'])

# Plot the sales trend
plt.figure(figsize=(14, 7))
sns.lineplot(data=sales_trend_df, x='year_month', y='total_revenue', marker='o')
plt.title('Monthly Sales Trend (Total Revenue)')
plt.xlabel('Month')
plt.ylabel('Total Revenue')
plt.grid(True)
plt.xticks(rotation=45)
plt.tight_layout()
plt.show()
```
![Alt image](https://github.com/yanheinaung23-eng/E-commerce-Sales-and-Customers-RFM-Analysis-Full-Project/blob/afd55405e504eea12b3781312fb40a6f91be59fa/EDA/Documents/Sales%20Trend.png)

### Monthly Order Trend
```python
monthly_order_trend_df = pd.read_sql_query(
    """SELECT
        strftime('%Y-%m', InvoiceDate) AS YearMonth,
        COUNT(DISTINCT InvoiceNo) AS TotalOrders
    FROM online_sales
    WHERE InvoiceNo NOT LIKE 'C%'
    GROUP BY YearMonth
    ORDER BY YearMonth;""",
    conn
)

# Convert 'YearMonth' to datetime for proper plotting
monthly_order_trend_df['YearMonth'] = pd.to_datetime(monthly_order_trend_df['YearMonth'])

# Plot the monthly order trend
plt.figure(figsize=(14, 7))
sns.lineplot(data=monthly_order_trend_df, x='YearMonth', y='TotalOrders', marker='o', color='orange')
plt.title('Monthly Order Trend')
plt.xlabel('Month')
plt.ylabel('Total Orders')
plt.grid(True)
plt.xticks(rotation=45)
plt.tight_layout()
plt.show()
```
![Alt image](https://github.com/yanheinaung23-eng/E-commerce-Sales-and-Customers-RFM-Analysis-Full-Project/blob/afd55405e504eea12b3781312fb40a6f91be59fa/EDA/Documents/Orders%20Trend.png)

### Monthly Active Customers Trend

```python
monthly_customers_trend_df = pd.read_sql_query(
    """SELECT
        strftime('%Y-%m', InvoiceDate) AS YearMonth,
        COUNT(DISTINCT CustomerID) AS ActiveCustomers
    FROM online_sales
    WHERE CustomerID IS NOT NULL
        AND InvoiceNo NOT LIKE 'C%'
    GROUP BY YearMonth
    ORDER BY YearMonth;""",
    conn
)

# Convert 'YearMonth' to datetime for proper plotting
monthly_customers_trend_df['YearMonth'] = pd.to_datetime(monthly_customers_trend_df['YearMonth'])

# Plot the monthly customer trend
plt.figure(figsize=(14, 7))
sns.lineplot(data=monthly_customers_trend_df, x='YearMonth', y='ActiveCustomers', marker='o', color='purple')
plt.title('Monthly Active Customers Trend')
plt.xlabel('Month')
plt.ylabel('Active Customers')
plt.grid(True)
plt.xticks(rotation=45)
plt.tight_layout()
plt.show()
```
![Alt image](https://github.com/yanheinaung23-eng/E-commerce-Sales-and-Customers-RFM-Analysis-Full-Project/blob/afd55405e504eea12b3781312fb40a6f91be59fa/EDA/Documents/Active%20Customers%20Trend.png)

***Insights***

Beginning in September, sales accelerated significantly, reaching a peak of **£1.50 million** in November 2011. This pattern suggests **strong seasonality** and increased customer demand during the holiday shopping period. Same trends in orders, customers confirm the strong seasonality. The dataset contain only partial December data and the apparent decline in December should not be interpreted.



