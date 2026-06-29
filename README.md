# 🛒 E-Commerce Sales & RFM Analysis

> **Comprehensive Business Intelligence Study** | TATA Group Virtual Internship · Completed June 2026

[![Tableau VOTD](https://img.shields.io/badge/🏆_Tableau-Viz_of_the_Day_Winner-E97627?style=for-the-badge&logo=tableau&logoColor=white)](https://public.tableau.com/app/profile/yan.aung3461/viz/E-CommerceSalesandRFMDashboardTATAinternshipProject/Dashboard1)
[![SQLite](https://img.shields.io/badge/SQLite-Database-003B57?style=for-the-badge&logo=sqlite&logoColor=white)](https://www.sqlite.org/)
[![Python](https://img.shields.io/badge/Python-Analytics-3776AB?style=for-the-badge&logo=python&logoColor=white)](https://www.python.org/)
[![Excel](https://img.shields.io/badge/Excel-Power_Query-217346?style=for-the-badge&logo=microsoft-excel&logoColor=white)](https://microsoft.com/excel)
[![Tableau](https://img.shields.io/badge/Tableau-Visualization-E97627?style=for-the-badge&logo=tableau&logoColor=white)](https://tableau.com/)
[![Google Colab](https://img.shields.io/badge/Google_Colab-Notebook-F9AB00?style=for-the-badge&logo=googlecolab&logoColor=white)](https://colab.research.google.com/)
[![License: MIT](https://img.shields.io/badge/License-MIT-green?style=for-the-badge)](LICENSE)

---

## 📌 Table of Contents

1. [Project Overview](#-project-overview)
2. [Award & Recognition](#tableau-viz-of-the-day-winner)
3. [Live Dashboard](#-live-dashboard)
4. [Tech Stack](#tech-stack)
5. [Analysis Workflow](#-analysis-workflow)
6. [Stakeholder Questions & Analysis](#-stakeholder-questions--analysis)
   - [Q1 — Seasonality: Local vs. International Markets](#q1--seasonality-local-vs-international-markets)
   - [Q2 — Revenue Concentration Risk & Expansion Targets](#q2--revenue-concentration-risk--expansion-targets)
   - [Q3 — VIP Customer Segmentation via RFM](#q3--vip-customer-segmentation-via-rfm)
   - [Q4 — Customer Retention & Repeat Purchase Rate](#q4--customer-retention--repeat-purchase-rate)
7. [Key Findings Summary](#-key-findings-summary)
8. [Data Source](#-data-source)
9. [Acknowledgements](#-acknowledgements)
10. [Connect](#-connect)

---

## 🎯 Project Overview

This project is a **full-stack business intelligence study** conducted for the TATA Group Data Visualisation Virtual Internship on Forage. It evaluates **global market performance**, **customer loyalty**, and **revenue risk** for a UK-based e-commerce retailer operating across international markets.

The analysis answers **four strategic questions** posed by business stakeholders, progressing from data cleaning all the way through advanced RFM segmentation and cohort-based retention modelling.

### Business Objectives

| Objective | Description |
|-----------|-------------|
| **Market Intelligence** | Identify seasonal revenue patterns across UK and international markets |
| **Risk Assessment** | Evaluate revenue concentration risk and pinpoint expansion opportunities |
| **Customer Segmentation** | Isolate premium VIP customers using RFM (Recency, Frequency, Monetary) scoring |
| **Retention Analysis** | Measure cohort retention rates and diagnose customer drop-off patterns |

---

## Tableau Viz of the Day Winner (VOTD) 🏆

This project's accompanying Tableau dashboard was awarded **Viz of the Day (VOTD)** by Tableau Public — a recognition given to dashboards that demonstrate exceptional design, analytical clarity, and storytelling.

---

## 📊 Live Dashboard

[![Dashboard Preview](https://github.com/yanheinaung23-eng/E-commerce-Sales-and-Customers-RFM-Analysis-Full-Project/blob/2961787e5b917c223cefc82b6b4587cb3241dbba/Documents/Tableau%20Dashboard.png)](https://public.tableau.com/app/profile/yan.aung3461/viz/E-CommerceSalesandRFMDashboardTATAinternshipProject/Dashboard1)

> 🔗 **[View the interactive dashboard on Tableau Public →](https://public.tableau.com/app/profile/yan.aung3461/viz/E-CommerceSalesandRFMDashboardTATAinternshipProject/Dashboard1)**

---

## Tech Stack

| Layer | Tool | Purpose |
|-------|------|---------|
| Data Cleaning | Microsoft Excel (Power Query) | ETL, deduplication, type casting |
| SQL Environment | SQLite in Google Colab | In-notebook SQL querying |
| Analysis | Python (pandas, matplotlib, seaborn) | EDA, visualisation, cohort modelling |
| Visualisation | Tableau Public | Interactive dashboard & storytelling |

---

## 🔄 Analysis Workflow

The project follows a structured, reproducible pipeline from raw data to executive insight. Each step is documented in its own README.

| Step | Stage | Documentation |
|------|-------|--------------|
| **1** | ![Data Cleaning](https://github.com/yanheinaung23-eng/E-commerce-Sales-and-Customers-RFM-Analysis-Full-Project/blob/bdf5ff1b196f917721fb1e718e015429c490fd71/Data%20Cleaning%20in%20Power%20Query/Documents/Data%20Cleaning%20with%20Excel%20Power%20Query.png) — **Data Cleaning with Excel Power Query** | [📄 README](https://github.com/yanheinaung23-eng/E-commerce-Sales-and-Customers-RFM-Analysis-Full-Project/tree/bdf5ff1b196f917721fb1e718e015429c490fd71/Data%20Cleaning%20in%20Power%20Query) |
| **2** | ![SQL Setup](https://github.com/yanheinaung23-eng/E-commerce-Sales-and-Customers-RFM-Analysis-Full-Project/blob/a98c00837ee17f48274bec0710765f298ed884f6/Documents/SQL%20Environment%20Setup%20in%20Google%20Colab.png) — **SQL Environment Setup in Google Colab** | [📄 README](https://github.com/yanheinaung23-eng/E-commerce-Sales-and-Customers-RFM-Analysis-Full-Project/tree/a98c00837ee17f48274bec0710765f298ed884f6/SQL%20Environment%20Setup%20in%20Google%20Colab) |
| **3** | ![DQA](https://github.com/yanheinaung23-eng/E-commerce-Sales-and-Customers-RFM-Analysis-Full-Project/blob/b81b10604cc6865d0a60e990308eb67c76b1636e/Data%20Quality%20Assessment/Documents/Data%20Quality%20Assessment.png) — **Data Quality Assessment** | [📄 README](https://github.com/yanheinaung23-eng/E-commerce-Sales-and-Customers-RFM-Analysis-Full-Project/tree/b81b10604cc6865d0a60e990308eb67c76b1636e/Data%20Quality%20Assessment) |
| **4** | ![EDA](https://github.com/yanheinaung23-eng/E-commerce-Sales-and-Customers-RFM-Analysis-Full-Project/blob/b81b10604cc6865d0a60e990308eb67c76b1636e/EDA/Documents/Exploratory%20Data%20Analysis.png) — **Exploratory Data Analysis (EDA)** | [📄 README](https://github.com/yanheinaung23-eng/E-commerce-Sales-and-Customers-RFM-Analysis-Full-Project/tree/b81b10604cc6865d0a60e990308eb67c76b1636e/EDA) |
| **5** | ![Advanced](https://github.com/yanheinaung23-eng/E-commerce-Sales-and-Customers-RFM-Analysis-Full-Project/blob/afa9b1658b562bfeae6ec5b81952e3a8ee8e6600/Documents/Advanced%20Analysis%20%E2%80%94%20Answering%20Stakeholders%20Questions.png) — **Advanced Analysis: Answering Stakeholder Questions** | 👇 Detailed below |

---

## 📐 Stakeholder Questions & Analysis

Four strategic questions were posed by business stakeholders. Each section below presents the SQL/Python code used, the resulting visualisation, key insights, and a concrete recommendation.

---

### Q1 — Seasonality: Local vs. International Markets

**Business Question:** *Does seasonality behave the same way across local and international markets?*

#### UK Monthly Sales Trend

```python
uk_sales_seasonality_df = pd.read_sql_query(
    """
    SELECT
        strftime('%Y-%m', InvoiceDate) AS year_month,
        ROUND(SUM(Revenue), 2)         AS total_revenue
    FROM online_sales
    WHERE Country = 'United Kingdom'
    GROUP BY strftime('%Y-%m', InvoiceDate)
    ORDER BY strftime('%Y-%m', InvoiceDate);
    """,
    conn
)

uk_sales_seasonality_df['year_month'] = pd.to_datetime(uk_sales_seasonality_df['year_month'])

plt.figure(figsize=(8, 8))
sns.lineplot(
    data=uk_sales_seasonality_df,
    x='year_month', y='total_revenue',
    marker='o', color='steelblue'
)
plt.title('UK Monthly Sales Seasonality', fontsize=16)
plt.xlabel('Month', fontsize=12)
plt.ylabel('Total Revenue', fontsize=12)
plt.grid(True)
plt.xticks(rotation=45)
plt.tight_layout()
plt.show()
```

#### International Monthly Sales Trend

```python
international_sales_seasonality_df = pd.read_sql_query(
    """
    SELECT
        strftime('%Y-%m', InvoiceDate) AS year_month,
        ROUND(SUM(Revenue), 2)         AS total_revenue
    FROM online_sales
    WHERE Country != 'United Kingdom'
    GROUP BY strftime('%Y-%m', InvoiceDate)
    ORDER BY strftime('%Y-%m', InvoiceDate);
    """,
    conn
)

international_sales_seasonality_df['year_month'] = pd.to_datetime(
    international_sales_seasonality_df['year_month']
)

plt.figure(figsize=(8, 8))
sns.lineplot(
    data=international_sales_seasonality_df,
    x='year_month', y='total_revenue',
    marker='o', color='orange'
)
plt.title('International Monthly Sales Seasonality', fontsize=16)
plt.xlabel('Month', fontsize=12)
plt.ylabel('Total Revenue', fontsize=12)
plt.grid(True)
plt.xticks(rotation=45)
plt.tight_layout()
plt.show()
```

![UK and International Monthly Sales Seasonality](https://github.com/yanheinaung23-eng/E-commerce-Sales-and-Customers-RFM-Analysis-Full-Project/blob/0fcbafef0d947a285dd47124098860a129e110da/Documents/UK%20and%20International%20Monthly%20Sales%20Seasonality.png)

#### 💡 Insights

- **Different peak months:** International markets peak in **October** — a full month earlier than the UK's **November** peak — driven by global buyers securing inventory ahead of holiday shipping deadlines.
- **Diverging January behaviour:** While the UK experiences a severe post-holiday slump in January, international markets show a significant demand surge, completely bucking the seasonal trend.

#### ✅ Recommendation

Decouple UK and international marketing calendars immediately. Launch international wholesale and promotional campaigns in early autumn, and leverage international channels as a high-performing clearance vehicle for excess stock each January.

---

### Q2 — Revenue Concentration Risk & Expansion Targets

**Business Question:** *Are we facing revenue concentration risk? Which countries should we prioritise for expansion?*

#### Regional Revenue & Average Order Value (AOV)

```python
revenue_concentration_df = pd.read_sql_query(
    """
    WITH order_summary AS (
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
        ROUND(SUM(OrderRevenue), 2)                                       AS TotalSales,
        COUNT(DISTINCT InvoiceNo)                                          AS TotalOrders,
        ROUND(SUM(OrderRevenue) * 1.0 / COUNT(DISTINCT InvoiceNo), 2)    AS AOV
    FROM order_summary
    GROUP BY Region;
    """,
    conn
)

revenue_concentration_df = revenue_concentration_df.sort_values(by='Region', ascending=True)
colors = ['orange', 'steelblue']

plt.figure(figsize=(6, 6))
plt.pie(
    revenue_concentration_df['TotalSales'],
    labels=revenue_concentration_df['Region'],
    autopct='%1.1f%%',
    startangle=90,
    colors=colors
)
plt.title('Total Sales Distribution by Region', fontsize=16)
plt.axis('equal')
plt.show()
```

![Total Sales Distribution By Region](https://github.com/yanheinaung23-eng/E-commerce-Sales-and-Customers-RFM-Analysis-Full-Project/blob/2fda2a7092294d9cfc5ff421af18853194e52489/Documents/Total%20Sales%20Distribution%20By%20Region.png)

| Region | Total Sales | Total Orders | AOV |
|--------|-------------|--------------|-----|
| UK | £8.18 million | 21,393 | £381.77 |
| International | £1.56 million | 2,405 | £648.18 |

#### Pareto Analysis — International Revenue by Country

```python
international_countries_revenue_df = pd.read_sql_query(
    """
    SELECT
        Country,
        ROUND(SUM(Revenue), 2) AS total_revenue
    FROM online_sales
    WHERE Country != 'United Kingdom'
    GROUP BY Country
    ORDER BY total_revenue DESC;
    """,
    conn
)

# Compute cumulative Pareto percentages
international_countries_revenue_df['cumulative_revenue']   = international_countries_revenue_df['total_revenue'].cumsum()
international_countries_revenue_df['cumulative_percentage'] = (
    100 * international_countries_revenue_df['cumulative_revenue']
        / international_countries_revenue_df['total_revenue'].sum()
)

fig, ax1 = plt.subplots(figsize=(14, 8))

sns.barplot(x='Country', y='total_revenue', data=international_countries_revenue_df, ax=ax1, color='orange')
ax1.set_xlabel('Country', fontsize=12)
ax1.set_ylabel('Total Revenue', fontsize=12)
ax1.tick_params(axis='x', rotation=90)

ax2 = ax1.twinx()
sns.lineplot(
    x='Country', y='cumulative_percentage',
    data=international_countries_revenue_df,
    ax=ax2, color='red', marker='o', sort=False
)
ax2.set_ylabel('Cumulative Percentage (%)', color='red', fontsize=12)
ax2.tick_params(axis='y', labelcolor='red')
ax2.set_ylim(0, 105)
ax2.axhline(80, color='red', linestyle='--', label='80% Threshold')
ax2.text(len(international_countries_revenue_df) - 1, 80, '80%', color='red', ha='right', va='bottom')

plt.title('Pareto Chart of International Revenue by Country', fontsize=16)
fig.tight_layout()
plt.show()
```

![Pareto Chart of International Revenue by Country](https://github.com/yanheinaung23-eng/E-commerce-Sales-and-Customers-RFM-Analysis-Full-Project/blob/a4e46b5eac4e6cc903671c9bf67487da1b438f57/Documents/Pareto%20Chart%20of%20International%20Revenue%20by%20Country.png)

#### 💡 Insights

- **Concentration risk confirmed:** 84% of total revenue originates from the UK, representing a significant single-market dependency.
- **International AOV advantage:** Despite lower total volume, international orders command a **71% higher AOV** (£648 vs. £382), indicating stronger per-order value.
- **Top expansion targets:** Applying the Pareto principle, **Netherlands, EIRE, Germany, and France** collectively account for **62% of international revenue** — all within the EU.

#### ✅ Recommendation

Reduce UK revenue dependency from 84% toward a **70–75% target** over 12–18 months. Develop country-specific growth plans for the Netherlands, EIRE, Germany, and France, focusing on tailored localisation and logistics improvements.

---

### Q3 — VIP Customer Segmentation via RFM

**Business Question:** *We want to run an exclusive VIP marketing campaign. Identify high-value customers based on Recency, Frequency, and Monetary.*

#### RFM Scoring Query

```sql
%%sql

WITH base AS (
    SELECT
        CustomerID,
        CASE WHEN Country = 'United Kingdom' THEN 'UK' ELSE 'International' END AS region,
        MIN(InvoiceDate)       AS first_purchase_date,
        MAX(InvoiceDate)       AS last_purchase_date,
        ROUND(SUM(Quantity), 2) AS total_quantity,
        COUNT(*)               AS Frequency,
        ROUND(SUM(Revenue), 2) AS Monetary
    FROM online_sales
    WHERE CustomerID IS NOT NULL
    GROUP BY CustomerID, Country
),
max_date AS (
    SELECT MAX(InvoiceDate) AS Maximum_date
    FROM online_sales
)
SELECT
    b.CustomerID,
    b.region,
    b.first_purchase_date,
    b.last_purchase_date,
    b.total_quantity,
    CAST(julianday(m.Maximum_date) - julianday(b.last_purchase_date) AS INTEGER) AS Recency,
    b.Frequency,
    b.Monetary,

    -- Recency Score: lower days since last purchase = higher score
    CASE
        WHEN (julianday(m.Maximum_date) - julianday(b.last_purchase_date)) BETWEEN 0  AND 14  THEN 5
        WHEN (julianday(m.Maximum_date) - julianday(b.last_purchase_date)) BETWEEN 15 AND 45  THEN 4
        WHEN (julianday(m.Maximum_date) - julianday(b.last_purchase_date)) BETWEEN 46 AND 90  THEN 3
        WHEN (julianday(m.Maximum_date) - julianday(b.last_purchase_date)) BETWEEN 91 AND 180 THEN 2
        ELSE 1
    END AS R_score,

    -- Frequency Score: more orders = higher score
    CASE
        WHEN b.Frequency <= 10  THEN 1
        WHEN b.Frequency <= 39  THEN 2
        WHEN b.Frequency <= 89  THEN 3
        WHEN b.Frequency <= 149 THEN 4
        ELSE 5
    END AS F_score,

    -- Monetary Score: higher spend = higher score
    CASE
        WHEN b.Monetary <= 500   THEN 1
        WHEN b.Monetary <= 2099  THEN 2
        WHEN b.Monetary <= 5000  THEN 3
        WHEN b.Monetary <= 14999 THEN 4
        ELSE 5
    END AS M_score

FROM base b
CROSS JOIN max_date m

-- Remove this filter to retrieve all customers with their full RFM scores
WHERE R_score = 5 AND F_score = 5 AND M_score = 5;
```

#### How RFM Scoring Works

This query establishes each customer's baseline RFM metrics against the dataset's most recent transaction date using SQLite's `julianday()` function, then maps each metric to a standardised **1–5 scale**.

| Dimension | Score = 5 Threshold | Meaning |
|-----------|---------------------|---------|
| **Recency** | Purchased within the last **14 days** | Actively engaged |
| **Frequency** | More than **149 orders** | Highly loyal |
| **Monetary** | Spent over **£14,999** | Premium spender |

> Applying `WHERE R_score = 5 AND F_score = 5 AND M_score = 5` isolates the **perfect 5-5-5 tier** — customers who are simultaneously the most recent, most frequent, and highest-spending — providing a tightly refined VIP target list for the exclusive marketing campaign.

---

### Q4 — Customer Retention & Repeat Purchase Rate

**Business Question:** *Analyse retention rates for customers and the overall repeat purchase rate.*

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
        CAST(SUM(CASE WHEN active_days > 1 THEN 1 ELSE 0 END) AS FLOAT)
        / COUNT(*) * 100,
        2
    ) || '%' AS overall_retention_rate
FROM customer_stats;
```

| Total Customers | Retained Customers | Overall Retention Rate |
|-----------------|-------------------|------------------------|
| 4,371 | 2,991 | **68.43%** |

#### Cohort Retention Heatmap

```python
cohort_raw = pd.read_sql_query("""
    WITH customer_first_purchase AS (
        SELECT
            CustomerID,
            strftime('%Y-%m', MIN(InvoiceDate)) AS cohort_month
        FROM online_sales
        WHERE CustomerID IS NOT NULL
        GROUP BY CustomerID
    ),
    customer_activity_months AS (
        SELECT DISTINCT
            CustomerID,
            strftime('%Y-%m', InvoiceDate) AS activity_month
        FROM online_sales
        WHERE CustomerID IS NOT NULL
    ),
    cohort_index_calc AS (
        SELECT
            a.CustomerID,
            c.cohort_month,
            (CAST(strftime('%Y', a.activity_month || '-01') AS INTEGER)
             - CAST(strftime('%Y', c.cohort_month || '-01') AS INTEGER)) * 12
            + (CAST(strftime('%m', a.activity_month || '-01') AS INTEGER)
               - CAST(strftime('%m', c.cohort_month || '-01') AS INTEGER)) AS cohort_index
        FROM customer_activity_months a
        JOIN customer_first_purchase c ON a.CustomerID = c.CustomerID
    )
    SELECT cohort_month, cohort_index, COUNT(DISTINCT CustomerID) AS unique_customers
    FROM cohort_index_calc
    GROUP BY cohort_month, cohort_index;
""", conn)

# Pivot to cohort matrix and normalise by initial cohort size
cohort_pivot    = cohort_raw.pivot(index='cohort_month', columns='cohort_index', values='unique_customers')
cohort_sizes    = cohort_pivot.iloc[:, 0]
retention_matrix = cohort_pivot.divide(cohort_sizes, axis=0)

# Render retention heatmap
plt.figure(figsize=(16, 10))
plt.title('Customer Retention Rate Cohort Analysis', fontsize=16, pad=20, weight='bold')

sns.heatmap(
    data=retention_matrix,
    annot=True,
    fmt='.1%',
    cmap='Blues',
    vmax=0.5,
    cbar_kws={'label': 'Retention Rate (%)'},
    linewidths=0.5,
    linecolor='#e2e8f0'
)

plt.xlabel('Cohort Index (Months Since Acquisition)', fontsize=12, labelpad=12)
plt.ylabel('Cohort Birth Month', fontsize=12, labelpad=12)
plt.tight_layout()
plt.show()
```

![Customer Retention Rate Cohort Analysis](https://github.com/yanheinaung23-eng/E-commerce-Sales-and-Customers-RFM-Analysis-Full-Project/blob/a4e46b5eac4e6cc903671c9bf67487da1b438f57/Documents/Customer%20Retention%20Rate%20Cohort%20Analysis.png)

#### 💡 Insights

- **Headline retention of 68.43%** is primarily anchored by the **December 2010 cohort**, which sustains a consistent **33%–40%** long-term retention rate and spikes to **50%** by Month 11.
- **"Month 1 Cliff" problem:** Subsequent 2011 cohorts experience a sharp drop into the **sub-25%** range immediately after acquisition — a critical vulnerability in the customer lifecycle.
- **Stabilisation pattern:** Customers who survive the initial month settle into a predictable recurring baseline of **20%–28%**, with natural re-engagement surges during the November holiday season.

#### ✅ Recommendation

Shift investment from top-of-funnel acquisition toward aggressive **Month 1 lifecycle marketing** (onboarding sequences, early incentive offers) to reduce the post-acquisition cliff. Simultaneously, deploy targeted **Q4 win-back campaigns** to capitalise on natural seasonal purchasing habits.

---

## 📋 Key Findings Summary

| # | Strategic Question | Core Finding | Recommendation |
|---|-------------------|--------------|----------------|
| **Q1** | Seasonality | International peaks in **October**; UK peaks in **November**. International revenue surges in January while UK slumps. | Decouple campaign calendars. Use January international channels for stock clearance. |
| **Q2** | Revenue Risk | **84%** of revenue from UK — high concentration risk. Netherlands, EIRE, Germany & France drive **62%** of international revenue. | Reduce UK share toward **70–75%**. Launch country-specific EU expansion plans. |
| **Q3** | VIP Customers | Perfect 5-5-5 RFM tier: purchased within **14 days**, **149+ orders**, **£14,999+ spend**. | Target this tier with exclusive VIP campaigns using the refined RFM query. |
| **Q4** | Retention | **68.43%** overall retention, anchored by the Dec 2010 cohort. New 2011 cohorts drop sharply after Month 1. | Invest in Month 1 lifecycle marketing and Q4 win-back campaigns. |

---

## 📁 Data Source

| Attribute | Detail |
|-----------|--------|
| **Dataset** | Online Retail II — UCI Machine Learning Repository |
| **Program** | [TATA Data Visualisation: Empowering Business with Effective Insights](https://www.theforage.com/simulations/tata/data-visualisation-p5xo) via Forage |
| **Period Covered** | December 2010 – December 2011 |

---

## 🙏 Acknowledgements

This project was completed as part of the **TATA Group Virtual Internship Job Simulation** hosted on [Forage](https://www.theforage.com/). Special thanks to TATA for designing a simulation that mirrors real-world, stakeholder-driven analytical challenges.

---

## 🤝 Connect

[![LinkedIn](https://img.shields.io/badge/LinkedIn-Connect-0A66C2?style=for-the-badge&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/yanheinaung/)
[![Tableau Public](https://img.shields.io/badge/Tableau_Public-Portfolio-E97627?style=for-the-badge&logo=tableau&logoColor=white)](https://public.tableau.com/app/profile/yan.aung3461)
[![GitHub](https://img.shields.io/badge/GitHub-Profile-181717?style=for-the-badge&logo=github&logoColor=white)](https://github.com/yanheinaung23-eng)

---

## 📄 License

This project is licensed under the [MIT License](LICENSE).








