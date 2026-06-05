
### E-commerce Sales & Customers RFM Analysis Project — EDA
> An Exploratory Data Analysis on a real-world online retail dataset using SQL (SQLite) and Python, uncovering patterns in sales performance, product demand, and customer behaviour.

---
## 📋 Table of Contents

- [Tools Used](#Tools-Used)
- [EDA Walkthrough](#EDA-Walkthrough)
 - [Monthly Sales Trend](#Monthly-Sales-Trend)
 
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

---
### Top 20 Products by Total Revenue
```python
top_products_df = pd.read_sql_query(
    """SELECT
    Description,
    ROUND(SUM(Revenue),2) AS total_revenue
    FROM online_sales
    GROUP BY Description
    ORDER BY ROUND(SUM(Revenue),2) DESC
    LIMIT 20;""",
    conn
)


# Plotting the top 20 products by revenue
plt.figure(figsize=(14, 8))
sns.barplot(x='total_revenue', y='Description', data=top_products_df, palette='viridis')
plt.title('Top 20 Products by Total Revenue', fontsize=16)
plt.xlabel('Total Revenue', fontsize=12)
plt.ylabel('Product Description', fontsize=12)
plt.tight_layout()
plt.show()
```
![Alt image](https://github.com/yanheinaung23-eng/E-commerce-Sales-and-Customers-RFM-Analysis-Full-Project/blob/823867626306830bc49e3d057fa74d2ab949876b/EDA/Documents/Top%2020%20Products%20by%20Total%20Revenue.png)

***Insights***

* Many top selling products belong to Home Decor, Party & Celebration, Gift & Kitchenware and this suggests our customers are purchasing decorative and gifting products rather than necessities. 
* The top product, `DOTCOM POSTAGE` generated **£206,245** and it shows shipping charges alone generated a substantial amount of revenue.

---

### Total Revenue by Country
```python
sales_by_country_df = pd.read_sql_query(
    """SELECT
    Country,
    ROUND(SUM(Revenue),2) AS total_revenue
    FROM online_sales
    GROUP BY Country
    ORDER BY SUM(Revenue) DESC;""",
    conn
)

# Plotting the sales by country
plt.figure(figsize=(14, 8))
sns.barplot(x='total_revenue', y='Country', data=sales_by_country_df, palette='viridis')
plt.title('Total Revenue by Country', fontsize=16)
plt.xlabel('Total Revenue', fontsize=12)
plt.ylabel('Country', fontsize=12)
plt.tight_layout()
plt.show()
```
![Alt image](https://github.com/yanheinaung23-eng/E-commerce-Sales-and-Customers-RFM-Analysis-Full-Project/blob/38212aff242e763d15b5c64bf4f9ee053eb5be29/EDA/Documents/Total%20Revenue%20by%20Country.png)

***Insights***

The United Kingdom was the company's primary market, generating **£8.17 million** in revenue and accounting for approximately 84% of total sales. International markets contributed the remaining 16%, with the Netherlands, EIRE, Germany, France, and Australia emerging as the strongest performers.

---

### Top 20 Customers by Total Revenue
```python
customer_revenue_df = pd.read_sql_query(
    """SELECT
    CAST(CustomerID AS TEXT) AS CustomerID,
    ROUND(SUM(Revenue),2) AS total_revenue
    FROM online_sales
    WHERE CustomerID IS NOT NULL
    GROUP BY CustomerID
    ORDER BY total_revenue DESC
    LIMIT 20;""",
    conn
)

# Plotting the top 20 customers by revenue
plt.figure(figsize=(14, 8))
sns.barplot(x='CustomerID', y='total_revenue', hue='CustomerID', data=customer_revenue_df, palette='Oranges', legend=False)

# Calculate the average revenue for the top 20 customers
average_revenue = customer_revenue_df['total_revenue'].mean()

# Add a horizontal line for the average revenue
plt.axhline(y=average_revenue, color='gray', linestyle='--', linewidth=2, label=f'Average Revenue: {average_revenue:.2f}')

plt.title('Top 20 Customers by Total Revenue', fontsize=16)
plt.xlabel('Customer ID', fontsize=12)
plt.ylabel('Total Revenue', fontsize=12)
plt.xticks(rotation=45, ha='right') # Rotate CustomerID labels for better readability
plt.legend()
plt.tight_layout()
plt.show()
```
![Alt image](https://github.com/yanheinaung23-eng/E-commerce-Sales-and-Customers-RFM-Analysis-Full-Project/blob/38212aff242e763d15b5c64bf4f9ee053eb5be29/EDA/Documents/Top%2020%20Customers%20by%20Total%20Revenue.png)

***Insights***

Customer revenue was highly concentrated among a small number of high-value customers. The highest-spending customer generated **£279K** in revenue, while several customers exceeded £100K in total purchases. The sharp decline in revenue beyond the top-ranked customers indicates a long-tail distribution, suggesting that a small proportion of customers contribute disproportionately to overall sales. This pattern may reflect the presence of wholesale or business buyers and highlights potential customer concentration risk. These findings support the need for further customer segmentation through RFM and retention analysis.

---

### Total Revenue by Customer Type

```python
customer_type_revenue_df = pd.read_sql_query(
    """SELECT
    CustomerType,
    ROUND(SUM(Revenue),2) AS total_revenue
    FROM online_sales
    GROUP BY CustomerType;""",
    conn
)

# Plotting the pie chart
plt.figure(figsize=(8, 8))
plt.pie(
    customer_type_revenue_df['total_revenue'],
    labels=customer_type_revenue_df['CustomerType'],
    autopct='%1.1f%%', # Format for percentage labels
    startangle=90,
    colors=sns.color_palette('pastel')
)
plt.title('Total Revenue by Customer Type', fontsize=16)
plt.axis('equal') # Equal aspect ratio ensures that pie is drawn as a circle.
plt.show()
```
![Alt image](https://github.com/yanheinaung23-eng/E-commerce-Sales-and-Customers-RFM-Analysis-Full-Project/blob/38212aff242e763d15b5c64bf4f9ee053eb5be29/EDA/Documents/Total%20Revenue%20by%20Customer%20Type.png)

***Insights***

Registered customers generated **£8.28 million** in revenue, accounting for approximately 85.1% of total sales, while guest customers contributed **£1.45 million** (14.9%). This indicates that the business is primarily driven by identifiable customers, enabling meaningful customer-level analysis such as retention, cohort, and RFM segmentation.

---

### Top 20 Products by Total Quantity Sold

```python
top_products_quantity_df = pd.read_sql_query(
    """SELECT
    Description,
    SUM(Quantity) AS total_quantity
    FROM online_sales
    GROUP BY Description
    ORDER BY SUM(Quantity) DESC
    LIMIT 20;""",
    conn
)

# Plotting the top 20 products by total quantity
plt.figure(figsize=(14, 8))
sns.barplot(x='total_quantity', y='Description', hue='Description', data=top_products_quantity_df, palette=['green'], legend=False)
plt.title('Top 20 Products by Total Quantity Sold', fontsize=16)
plt.xlabel('Total Quantity Sold', fontsize=12)
plt.ylabel('Product Description', fontsize=12)
plt.tight_layout()
plt.show()
```
![Alt image](https://github.com/yanheinaung23-eng/E-commerce-Sales-and-Customers-RFM-Analysis-Full-Project/blob/38212aff242e763d15b5c64bf4f9ee053eb5be29/EDA/Documents/Top%2020%20Products%20by%20Quantity%20sold.png)

**Insights**

Product sales volume was led by `WORLD WAR 2 GLIDERS ASSTD DESIGNS` (53,751 units), followed by `JUMBO BAG RED RETROSPOT` (47,256 units) and `POPCORN HOLDER` (36,322 units). The ranking differs substantially from the top revenue-generating products, indicating that high sales volume does not necessarily translate into high revenue. Several products, including `JUMBO BAG RED RETROSPOT`, `WHITE HANGING HEART T-LIGHT HOLDER`, and `RABBIT NIGHT LIGHT`, performed strongly in both revenue and quantity sold, demonstrating both popularity and commercial value. Additionally, multiple Retrospot-themed products ranked among the top sellers, suggesting strong customer demand for this product family. Overall, the retailer's sales appear to be driven by a mix of high-volume, low-priced items and premium products that generate significant revenue despite lower sales quantities.

---

### Top 20 Most Frequent Registered Customers

```python
most_frequent_customers_df = pd.read_sql_query(
    """SELECT
    CustomerID,
    COUNT(*) AS total_count
    FROM online_sales
    WHERE CustomerType = 'Registered'
    GROUP BY CustomerID
    ORDER BY COUNT(*) DESC
    LIMIT 20;""",
    conn
)

# Convert CustomerID to string for better display on x-axis
most_frequent_customers_df['CustomerID'] = most_frequent_customers_df['CustomerID'].astype(str)

# Plotting the most frequent registered customers
plt.figure(figsize=(14, 8))
sns.barplot(x='CustomerID', y='total_count', data=most_frequent_customers_df, palette='GnBu_r', hue='CustomerID', legend=False)
plt.title('Top 20 Most Frequent Registered Customers', fontsize=16)
plt.xlabel('Customer ID', fontsize=12)
plt.ylabel('Total Transactions', fontsize=12)
plt.xticks(rotation=45, ha='right')
plt.tight_layout()
plt.show()
```
![Alt image](https://github.com/yanheinaung23-eng/E-commerce-Sales-and-Customers-RFM-Analysis-Full-Project/blob/38212aff242e763d15b5c64bf4f9ee053eb5be29/EDA/Documents/Top%2020%20Most%20Frequent%20Registered%20Customers.png)

**Insights**

Customer 17841 generated the highest number of transaction records **(7,812)**, indicating extensive purchasing activity and a large variety of products purchased. This metric reflects transaction volume rather than purchase frequency and may be influenced by larger basket sizes or wholesale purchasing behavior.

---

### One-Time vs Repeat Customers

```python
one_time_vs_repeat_df = pd.read_sql_query(
    """WITH customer_orders AS (
    SELECT
        CustomerID,
        COUNT(DISTINCT InvoiceNo) AS OrderCount
    FROM online_sales
    WHERE CustomerID IS NOT NULL
    GROUP BY CustomerID
    )

    SELECT
        CASE
            WHEN OrderCount = 1 THEN 'One-Time'
            ELSE 'Repeat'
        END AS CustomerType,
        COUNT(*) AS Customers
    FROM customer_orders
    GROUP BY CustomerType;""",
    conn
)

# Define custom colors: red for 'One-Time' and green for 'Repeat'
# Ensure the order matches the order of CustomerType in the DataFrame
custom_colors = ['red', 'green']

# Plotting the pie chart
plt.figure(figsize=(8, 8))
plt.pie(
    one_time_vs_repeat_df['Customers'],
    labels=one_time_vs_repeat_df['CustomerType'],
    autopct='%1.1f%%', # Format for percentage labels
    startangle=90,
    colors=custom_colors
)
plt.title('One-Time vs Repeat Customers', fontsize=16)
plt.axis('equal') # Equal aspect ratio ensures that pie is drawn as a circle.
plt.show()
```
![Alt image](https://github.com/yanheinaung23-eng/E-commerce-Sales-and-Customers-RFM-Analysis-Full-Project/blob/38212aff242e763d15b5c64bf4f9ee053eb5be29/EDA/Documents/One-time%20vs%20Repeat%20Customers.png)

**Insights**

Customer purchasing behavior was dominated by repeat buyers. Of the 4,371 registered customers, 3,059 (70%) placed more than one order, while 1,312 (30%) made only a single purchase. The high proportion of repeat customers suggests strong customer loyalty and indicates that **retention plays an important role in overall business performance.**

---

### Top 10 Countries by Number of Customers

```python
customer_by_country_df = pd.read_sql_query(
    """SELECT
        Country,
        COUNT(DISTINCT CustomerID) AS Customers
    FROM online_sales
    GROUP BY Country
    ORDER BY Customers DESC;""",
    conn
)

# Select the top 10 countries
top_10_countries = customer_by_country_df.head(10)

# Plotting the top 10 countries by customers
plt.figure(figsize=(12, 7))
sns.barplot(x='Customers', y='Country', data=top_10_countries, palette='viridis')
plt.title('Top 10 Countries by Number of Customers', fontsize=16)
plt.xlabel('Number of Customers', fontsize=12)
plt.ylabel('Country', fontsize=12)
plt.tight_layout()
plt.show()
```
![Alt image](https://github.com/yanheinaung23-eng/E-commerce-Sales-and-Customers-RFM-Analysis-Full-Project/blob/38212aff242e763d15b5c64bf4f9ee053eb5be29/EDA/Documents/Top%2010%20Countries%20by%20Number%20of%20Customers.png)

***Insights***

The customer base is heavily concentrated in the United Kingdom, which accounts for 3,949 customers (90.3% of all registered customers). International customers represent only 9.7% of the customer base but span more than 30 countries, demonstrating broad global reach. Germany and France emerged as the largest international markets by customer count, while countries such as the Netherlands, EIRE, and Australia generated substantial revenue despite relatively small customer bases.

---

## Insights Summary

### 📊 Revenue & Seasonality

* **The Holiday Spike**: Revenue explodes in September and peaks in November at £1.50 million, proving extreme holiday seasonality.
* **UK-Dominant**: The United Kingdom drives 84% of total sales (£8.17M), holding massive home-market concentration.
* **Hidden Logistics Value**: DOTCOM POSTAGE is the #1 revenue "product" (£206K), proving shipping fees heavily dictate cash flow.

### 🛍️ Product & Catalog Performance

* **Gifting & Impulse Core**: Top products favor Home Decor and Celebration items.
* **The Power Family**: "Retrospot" themed items dominate both volume and value, highlighting your strongest brand aesthetic.

### 👥 Customer Behavior & Loyalty

* **The Whale Trap**: High revenue concentration among the top 20 buyers hints at heavy business-to-business (B2B) or wholesale reliance.
* **Retention-business**: 70% of registered users are repeat buyers, proving excellent product-market fit and strong loyalty.
* **International Efficiency**: Overseas buyers match only 9.7% of headcount, but markets like the Netherlands and EIRE yield massive average order values.

---








