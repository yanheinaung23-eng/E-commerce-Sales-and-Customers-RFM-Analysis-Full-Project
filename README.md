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
![Alt image](https://github.com/yanheinaung23-eng/E-commerce-Sales-and-Customers-RFM-Analysis-Full-Project/blob/a4e46b5eac4e6cc903671c9bf67487da1b438f57/Documents/Pareto%20Chart%20of%20International%20Revenue%20by%20Country.png)

#### Insights
* The business currently faces revenue concentration risk because 84% of total sales come from the UK market. While international revenue has 71% higher AOV than UK, market is distributed across multiple countries, making it less risky than the UK dependence.

* According to Pareto principle, the strongest expansion opportunities are:
Netherlands, EIRE, Germany, France generating 62% of total international revenue and all countries are EU region.

#### Recommandation
* Reduce dependence on UK revenue from 84% toward 70–75%.
* Expand into EU countries with country-specific growth plans.

---
### Q3. We want to do exclusive VIPs marketing campaign. Identify the high-end customers based on their Recency, Frequency and Monetary.

```sql
%%sql

WITH base AS
(
  SELECT
    CustomerID,
    CASE
      WHEN Country = 'United Kingdom' THEN 'UK'
      ELSE 'International'
    END AS region,
    MIN(InvoiceDate) AS first_purchase_date,
    MAX(InvoiceDate) AS last_purchase_date,
    ROUND(SUM(Quantity), 2) AS total_quantity,
    COUNT(*) AS Frequency,
    ROUND(SUM(Revenue), 2) AS Monetary
  FROM online_sales
  WHERE CustomerID IS NOT NULL
  GROUP BY CustomerID, Country
),
max_date AS
(
  SELECT MAX(InvoiceDate) AS Maximum_date
  FROM online_sales
)
SELECT
    b.CustomerID,
    b.region,
    b.first_purchase_date,
    b.last_purchase_date,
    b.total_quantity,
    -- Calculate Recency using julianday for SQLite
    CAST(julianday(m.Maximum_date) - julianday(b.last_purchase_date) AS INTEGER) AS Recency,
    b.Frequency,
    b.Monetary,
    -- R_score Calculation
    CASE
        WHEN (julianday(m.Maximum_date) - julianday(b.last_purchase_date)) BETWEEN 0 AND 14 THEN 5
        WHEN (julianday(m.Maximum_date) - julianday(b.last_purchase_date)) BETWEEN 15 AND 45 THEN 4
        WHEN (julianday(m.Maximum_date) - julianday(b.last_purchase_date)) BETWEEN 46 AND 90 THEN 3
        WHEN (julianday(m.Maximum_date) - julianday(b.last_purchase_date)) BETWEEN 91 AND 180 THEN 2
        ELSE 1
    END AS R_score,
    -- F_score Calculation
    CASE
        WHEN b.Frequency <= 10 THEN 1
        WHEN b.Frequency <= 39 THEN 2
        WHEN b.Frequency <= 89 THEN 3
        WHEN b.Frequency <= 149 THEN 4
        ELSE 5
    END AS F_score,
    -- M_score Calculation
    CASE
        WHEN b.Monetary <= 500 THEN 1
        WHEN b.Monetary <= 2099 THEN 2
        WHEN b.Monetary <= 5000 THEN 3
        WHEN b.Monetary <= 14999 THEN 4
        ELSE 5
    END AS M_score
FROM base b
CROSS JOIN max_date m
WHERE R_score = 5 AND F_score = 5 AND M_score = 5; -- Remove this step to get all the customer lists with different score
```
#### RFM Table Step by Step

Calculated customers baseline RFM metrics against the dataset's maximum date using SQLite's `julianday` function, and maps those metrics onto a standardized 1-to-5 scoring system.
 
 High-end status is dynamically defined by specific operational thresholds:
  * buying within the last 14 days (Recency = 5),
 *  placing more than 149 orders (Frequency = 5),
 *  and spending over £14,999 (Monetary = 5).
 
  By applying the final WHERE clause to filter exclusively for customers who score a perfect 5 across all three dimensions, the query isolates the most active, loyal, and highest-spending tier, providing a highly refined list of premium targets tailored perfectly for an exclusive VIP marketing campaign.

---

### Q4. Analyze retention rates for customers and overall repeat purchase rate.

#### Overall Retention Rate
```sql
%%sql

WITH customer_stats AS (
    SELECT
        CustomerID,
        COUNT(DISTINCT strftime('%Y-%m-%d', InvoiceDate)) AS active_days
    FROM online_sales
    WHERE CustomerID IS NOT NULL
    GROUP BY CustomerID
)
SELECT
    COUNT(*) AS total_customers,
    SUM(CASE WHEN active_days > 1 THEN 1 ELSE 0 END) AS retained_customers,
    ROUND(
        CAST(SUM(CASE WHEN active_days > 1 THEN 1 ELSE 0 END) AS FLOAT) / COUNT(*) * 100,
        2
    ) || '%' AS overall_retention_rate
FROM customer_stats;
```
| Total_customers | Retained_customers | Overall_retention_rate |
|---|---|---|
| 4371 | 2991 | 68.43% |

#### Customer Retention Rate Cohort Analysis
```python
cohort_raw = pd.read_sql_query("""
    WITH customer_first_purchase AS (
        SELECT CustomerID, strftime('%Y-%m', MIN(InvoiceDate)) AS cohort_month
        FROM online_sales
        WHERE CustomerID IS NOT NULL
        GROUP BY CustomerID
    ),
    customer_activity_months AS (
        SELECT DISTINCT CustomerID, strftime('%Y-%m', InvoiceDate) AS activity_month
        FROM online_sales
        WHERE CustomerID IS NOT NULL
    ),
    cohort_index_calc AS (
        SELECT
            a.CustomerID,
            c.cohort_month,
            (CAST(strftime('%Y', a.activity_month || '-01') AS INTEGER) - CAST(strftime('%Y', c.cohort_month || '-01') AS INTEGER)) * 12 +
            (CAST(strftime('%m', a.activity_month || '-01') AS INTEGER) - CAST(strftime('%m', c.cohort_month || '-01') AS INTEGER)) AS cohort_index
        FROM customer_activity_months a
        JOIN customer_first_purchase c ON a.CustomerID = c.CustomerID
    )
    SELECT cohort_month, cohort_index, COUNT(DISTINCT CustomerID) AS unique_customers
    FROM cohort_index_calc
    GROUP BY cohort_month, cohort_index;
""", conn)

# 2. Pivot the flat table into a broad analytical matrix
cohort_pivot = cohort_raw.pivot(index='cohort_month', columns='cohort_index', values='unique_customers')

# 3. Save initial cohort sizes (Month 0) to compute percentages
cohort_sizes = cohort_pivot.iloc[:, 0]

# 4. Convert absolute user counts into relative retention rates (decimals)
retention_matrix = cohort_pivot.divide(cohort_sizes, axis=0)

# 5. Build and render the heatmap visualization
plt.figure(figsize=(16, 10))
plt.title('Customer Retention Rate Cohort Analysis', fontsize=16, pad=20, weight='bold')

sns.heatmap(
    data=retention_matrix,
    annot=True,
    fmt='.1%',              # Displays values as clean percentages (e.g., 15.4%)
    cmap='Blues',           # Classic sequential blue grading
    vmax=0.5,               # Caps color intensity at 50% to make subtle variations stand out
    cbar_kws={'label': 'Retention Rate (%)'},
    linewidths=0.5,
    linecolor='#e2e8f0'
)

# Label formatting to match image layout
plt.xlabel('Cohort Index (Months Passed)', fontsize=12, labelpad=12)
plt.ylabel('Cohort Birth Month', fontsize=12, labelpad=12)
plt.tight_layout()
plt.show()
```
![Alt image](https://github.com/yanheinaung23-eng/E-commerce-Sales-and-Customers-RFM-Analysis-Full-Project/blob/a4e46b5eac4e6cc903671c9bf67487da1b438f57/Documents/Customer%20Retention%20Rate%20Cohort%20Analysis.png)

#### Insights
Overall retention rate of 68.43% is primarily anchored by the exceptional long-term loyalty.

 December 2010 cohort, which consistently maintains a **33%** to **40%** retention rate and spikes to **50%** by Month 11. 
 
 However, there is an immediate "Month 1 Cliff" where subsequent 2011 cohorts experience a sharp drop-off into the sub-**25%** range right after acquisition.
 
  Fortunately, customers who survive this initial month stabilize into a highly predictable recurring baseline (**20%** to **28%**) for the rest of their lifecycle, with explicit re-engagement surges occurring during the November holiday season.

#### Recommandation
To maximize lifetime value, we must shift focus from top-of-funnel acquisition to aggressive Month 1 lifecycle marketing, while simultaneously deploying targeted Q4 win-back campaigns to capitalize on natural seasonal purchasing habits.

---

## 📊 Key Takeaways

| # | Question | Core Finding | Recommendation |
|---|---|---|---|
| Q1 | Seasonality | International peaks in October; UK peaks in November. International defies the January slump. | Decouple UK and international campaign calendars. Use January international channels for clearance. |
| Q2 | Revenue Risk | 84% revenue from UK — high concentration risk. Top EU markets (Netherlands, EIRE, Germany, France) drive 62% of international revenue. | Reduce UK dependency toward 70–75%. Launch country-specific EU expansion plans. |
| Q3 | VIP Customers | Customers with R=5, F=5, M=5: active within 14 days, 149+ orders, £14,999+ spend. | Target this tier with exclusive VIP campaigns using the refined RFM query. |
| Q4 | Retention | 68.43% overall retention, anchored by the Dec 2010 cohort. New 2011 cohorts drop sharply after Month 1. | Invest in Month 1 lifecycle marketing and Q4 win-back campaigns. |

---

## 📁 Data Source

- **Dataset:** Online Retail II — UCI Machine Learning Repository  
- **Program:** [TATA Data Visualisation: Empowering Business with Effective Insights](https://www.theforage.com/simulations/tata/data-visualisation-p5xo) via Forage  
- **Period Covered:** December 2010 – December 2011  

---

## 🙏 Acknowledgements

This project was completed as part of the **TATA Group Virtual Internship Job Simulation** hosted on [Forage](https://www.theforage.com/). Special thanks to TATA for designing a simulation that mirrors real-world stakeholder-driven analysis.

---

## 🤝 Let's Connect

[![LinkedIn](https://img.shields.io/badge/LinkedIn-Connect-0A66C2?style=for-the-badge&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/YOUR-LINKEDIN-HANDLE)
[![Tableau Public](https://img.shields.io/badge/Tableau%20Public-Portfolio-E97627?style=for-the-badge&logo=tableau&logoColor=white)](https://public.tableau.com/app/profile/yan.aung3461)
[![GitHub](https://img.shields.io/badge/GitHub-Profile-181717?style=for-the-badge&logo=github&logoColor=white)](https://github.com/yanheinaung23-eng)

---

## 📄 License

This project is licensed under the [MIT License](LICENSE).









