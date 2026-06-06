# E-commerce-Sales-and-Customers-RFM-Analysis-Full-Project

## Overview
This **award-winning** project, titled E-Commerce Sales and RFM Analysis , is a comprehensive business intelligence study developed for a [TATA virtual internship - Job Simulation Program](https://www.theforage.com/simulations/tata/data-visualisation-p5xo) to evaluate global market performance and customer loyalty. Completed in June 2026 , this project utilizes a robust technical stack—including setting up SQL environment in Google Colab in order to query, analyze and visualize all in one place. 

![Alt image](https://github.com/yanheinaung23-eng/E-commerce-Sales-and-Customers-RFM-Analysis-Full-Project/blob/2961787e5b917c223cefc82b6b4587cb3241dbba/Documents/Tableau%20Dashboard.png)

---

## Award
Tableau Viz of the Day Winner (VOTD) 🏆

[Check out my dashboard here!](https://public.tableau.com/app/profile/yan.aung3461/viz/E-CommerceSalesandRFMDashboardTATAinternshipProject/Dashboard1)

---

## Tools I used 🛠️
![SQLite](https://img.shields.io/badge/SQLite-Database-003B57?style=for-the-badge&logo=sqlite&logoColor=white)
![Excel](https://img.shields.io/badge/Excel-217346?style=for-the-badge&logo=microsoft-excel&logoColor=white)
![Tableau](https://img.shields.io/badge/Tableau-Visualization-E97627?style=for-the-badge&logo=tableau&logoColor=white)
![Google%20Colab](https://img.shields.io/badge/Google%20Colab-Notebook-F9AB00?style=for-the-badge&logo=googlecolab&logoColor=white)
![Python](https://img.shields.io/badge/Python-Analytics-3776AB?style=for-the-badge&logo=python&logoColor=white)
![Google%20Gemini](https://img.shields.io/badge/Google%20Gemini-AI%20Assistant-8E75FF?style=for-the-badge&logo=google-gemini&logoColor=white)

---
## 🔄 Analysis Workflow
This is my step by step workflow, please view each workflow by clicking README.md link. Enjoy!

| Step | Workflow | file |
|---|---|---|
| 1. | ![Alt image](https://github.com/yanheinaung23-eng/E-commerce-Sales-and-Customers-RFM-Analysis-Full-Project/blob/bdf5ff1b196f917721fb1e718e015429c490fd71/Data%20Cleaning%20in%20Power%20Query/Documents/Data%20Cleaning%20with%20Excel%20Power%20Query.png) | [README.md](https://github.com/yanheinaung23-eng/E-commerce-Sales-and-Customers-RFM-Analysis-Full-Project/tree/bdf5ff1b196f917721fb1e718e015429c490fd71/Data%20Cleaning%20in%20Power%20Query) |
| 2. | ![Alt image](https://github.com/yanheinaung23-eng/E-commerce-Sales-and-Customers-RFM-Analysis-Full-Project/blob/a98c00837ee17f48274bec0710765f298ed884f6/Documents/SQL%20Environment%20Setup%20in%20Google%20Colab.png) | [README.md](https://github.com/yanheinaung23-eng/E-commerce-Sales-and-Customers-RFM-Analysis-Full-Project/tree/a98c00837ee17f48274bec0710765f298ed884f6/SQL%20Environment%20Setup%20in%20Google%20Colab) |
| 3. | ![Alt image](https://github.com/yanheinaung23-eng/E-commerce-Sales-and-Customers-RFM-Analysis-Full-Project/blob/b81b10604cc6865d0a60e990308eb67c76b1636e/Data%20Quality%20Assessment/Documents/Data%20Quality%20Assessment.png) | [README.md](https://github.com/yanheinaung23-eng/E-commerce-Sales-and-Customers-RFM-Analysis-Full-Project/tree/b81b10604cc6865d0a60e990308eb67c76b1636e/Data%20Quality%20Assessment) |
| 4. | ![Alt image](https://github.com/yanheinaung23-eng/E-commerce-Sales-and-Customers-RFM-Analysis-Full-Project/blob/b81b10604cc6865d0a60e990308eb67c76b1636e/EDA/Documents/Exploratory%20Data%20Analysis.png) | [README.md](https://github.com/yanheinaung23-eng/E-commerce-Sales-and-Customers-RFM-Analysis-Full-Project/tree/b81b10604cc6865d0a60e990308eb67c76b1636e/EDA) |
| 5. | ![Alt image](https://github.com/yanheinaung23-eng/E-commerce-Sales-and-Customers-RFM-Analysis-Full-Project/blob/afa9b1658b562bfeae6ec5b81952e3a8ee8e6600/Documents/Advanced%20Analysis%20%E2%80%94%20Answering%20Stakeholders%20Questions.png) | 👇👇👇 |

---

## Answering Stakeholders Questions

### Q1. Does seasonality behave the same way across local and international markets?

#### UK sales Trend

```python
uk_sales_seasonality_df = pd.read_sql_query(
    """SELECT
        strftime('%Y-%m', InvoiceDate) AS year_month,
        ROUND(SUM(Revenue),2) AS total_revenue
    FROM online_sales
    WHERE Country = 'United Kingdom'
    GROUP BY strftime('%Y-%m', InvoiceDate)
    ORDER BY strftime('%Y-%m', InvoiceDate);""",
    conn
)

# Convert 'year_month' to datetime for proper plotting
uk_sales_seasonality_df['year_month'] = pd.to_datetime(uk_sales_seasonality_df['year_month'])

# Plotting the UK sales seasonality
plt.figure(figsize=(8, 8)) # Changed to a square figure size
sns.lineplot(data=uk_sales_seasonality_df, x='year_month', y='total_revenue', marker='o', color='steelblue') # Changed color to 'steelblue'
plt.title('UK Monthly Sales Seasonality', fontsize=16)
plt.xlabel('Month', fontsize=12)
plt.ylabel('Total Revenue', fontsize=12)
plt.grid(True)
plt.xticks(rotation=45)
plt.tight_layout()
plt.show()
```
#### International sales Trend
```python
international_sales_seasonality_df = pd.read_sql_query(
    """SELECT
        strftime('%Y-%m', InvoiceDate) AS year_month,
        ROUND(SUM(Revenue),2) AS total_revenue
    FROM online_sales
    WHERE Country != 'United Kingdom'
    GROUP BY strftime('%Y-%m', InvoiceDate)
    ORDER BY strftime('%Y-%m', InvoiceDate);""",
    conn
)

# Convert 'year_month' to datetime for proper plotting
international_sales_seasonality_df['year_month'] = pd.to_datetime(international_sales_seasonality_df['year_month'])

# Plotting the international sales seasonality
plt.figure(figsize=(8, 8)) # Make the chart square
sns.lineplot(data=international_sales_seasonality_df, x='year_month', y='total_revenue', marker='o', color='orange')
plt.title('International Monthly Sales Seasonality', fontsize=16)
plt.xlabel('Month', fontsize=12)
plt.ylabel('Total Revenue', fontsize=12)
plt.grid(True)
plt.xticks(rotation=45)
plt.tight_layout()
plt.show()
```
![Alt image](https://github.com/yanheinaung23-eng/E-commerce-Sales-and-Customers-RFM-Analysis-Full-Project/blob/0fcbafef0d947a285dd47124098860a129e110da/Documents/UK%20and%20International%20Monthly%20Sales%20Seasonality.png)

#### Insights

* International markets peak in October, a full month earlier than the UK’s November peak, driven by global buyers securing inventory ahead of holiday shipping constraints.
* Furthermore, while the UK suffers from a severe post-holiday slump in January, international sales buck the trend entirely with a massive demand surge.

#### Recommandation

To maximize profitability, the business must immediately decouple its strategies: target international wholesale and promotional campaigns early in the autumn, and use international channels as a high-performing clearance vehicle for excess stock every January.

---
### Q2. Are we having revenue concentration risk? Which countries should we expand?

#### Revenue concentration risk and AOV
```python
revenue_concentration_df = pd.read_sql_query(
    """WITH order_summary AS (
        SELECT
            InvoiceNo,
            CASE
                WHEN Country = 'United Kingdom' THEN 'UK'
                ELSE 'International'
            END AS Region,
            SUM(Revenue) AS OrderRevenue
        FROM online_sales
        GROUP BY InvoiceNo, Region
    )

    SELECT
        Region,
        ROUND(SUM(OrderRevenue), 2) AS TotalSales,
        COUNT(DISTINCT InvoiceNo) AS TotalOrders,
        ROUND(
            SUM(OrderRevenue) * 1.0 /
            COUNT(DISTINCT InvoiceNo),
            2
        ) AS AOV
    FROM order_summary
    GROUP BY Region;""",
    conn
)

# Sort DataFrame by Region to ensure consistent color mapping
revenue_concentration_df = revenue_concentration_df.sort_values(by='Region', ascending=True)
colors = ['orange', 'steelblue']

# Create the pie chart for TotalSales
plt.figure(figsize=(6, 6))
plt.pie(
    revenue_concentration_df['TotalSales'],
    labels=revenue_concentration_df['Region'],
    autopct='%1.1f%%', # Format for percentage labels
    startangle=90,
    colors=colors
)
plt.title('Total Sales Distribution by Region', fontsize=16)
plt.axis('equal')
plt.show()

# Display the data as a table
print("\nDetailed Revenue Concentration Metrics:")
display(revenue_concentration_df)
```
![Alt image](https://github.com/yanheinaung23-eng/E-commerce-Sales-and-Customers-RFM-Analysis-Full-Project/blob/2fda2a7092294d9cfc5ff421af18853194e52489/Documents/Total%20Sales%20Distribution%20By%20Region.png)

| Region | TotalSales | TotalOrders | AOV |
|---|---|---|---|
| International | 1.56 million | 2405 | 648.18 |
| UK | 8.18 million | 21393 | 381.77 |

#### Pareto Chart of International Revenue by Country

```python
international_countries_revenue_df = pd.read_sql_query(
    """SELECT
    Country,
    ROUND(SUM(Revenue),2) AS total_revenue
    FROM online_sales
    WHERE Country != 'United Kingdom'
    GROUP BY Country
    ORDER BY ROUND(SUM(Revenue),2) DESC;""",
    conn
)

# Calculate cumulative percentage
international_countries_revenue_df['cumulative_revenue'] = international_countries_revenue_df['total_revenue'].cumsum()
international_countries_revenue_df['cumulative_percentage'] = 100 * international_countries_revenue_df['cumulative_revenue'] / international_countries_revenue_df['total_revenue'].sum()

# Create the Pareto chart
fig, ax1 = plt.subplots(figsize=(14, 8))

# Bar plot for total revenue
sns.barplot(x='Country', y='total_revenue', data=international_countries_revenue_df, ax=ax1, color='orange')
ax1.set_xlabel('Country', fontsize=12)
ax1.set_ylabel('Total Revenue', fontsize=12)
ax1.tick_params(axis='x', rotation=90)

# Create a second y-axis for the cumulative percentage
ax2 = ax1.twinx()
sns.lineplot(x='Country', y='cumulative_percentage', data=international_countries_revenue_df, ax=ax2, color='red', marker='o', sort=False)
ax2.set_ylabel('Cumulative Percentage (%)', color='red', fontsize=12)
ax2.tick_params(axis='y', labelcolor='red')
ax2.set_ylim(0, 105) # Set y-limit to slightly above 100% for better visualization

# Add 80% line
ax2.axhline(80, color='red', linestyle='--', label='80% Line')
ax2.text(len(international_countries_revenue_df)-1, 80, '80%', color='red', ha='right', va='bottom')

plt.title('Pareto Chart of International Revenue by Country', fontsize=16)
fig.tight_layout()
plt.show()
```
#### Insights
* The business currently faces revenue concentration risk because 84% of total sales come from the UK market. While international revenue has 71% higher AOV than UK, market is distributed across multiple countries, making it less risky than the UK dependence.

* According to Pareto principle, the strongest expansion opportunities are:
Netherlands, EIRE, Germany, France generating 62% of total international revenue and all countries are EU region.

#### Recommandation
* Reduce dependence on UK revenue from 84% toward 70–75%.
* Expand into EU countries with country-specific growth plans.

---





