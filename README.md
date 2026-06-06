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


