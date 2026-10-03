# E-commerce-revenue-and-customer-satisfaction-analysis
Analysis of E-commerce revenue, customer satisfaction, sales performance, and delivery trends using excel and power bi.
# E-Commerce Revenue and Customer Satisfaction Analysis

An end-to-end analytics project that cleans and analyses an e-commerce order dataset in **Excel** and presents the results in an interactive **Power BI** dashboard. The goal is to show how revenue is generated and how satisfied customers are, and how region, product category, discounts and delivery time influence both.

![Power BI Dashboard](images/powerbi-dashboard.png)

---

## Table of Contents

- [Project Overview](#project-overview)
- [Business Questions](#business-questions)
- [Dataset](#dataset)
- [Tools and Technologies](#tools-and-technologies)
- [Data Pre-Processing (Excel)](#data-pre-processing-excel)
- [DAX Measures (Power BI)](#dax-measures-power-bi)
- [Dashboard](#dashboard)
- [Key Findings](#key-findings)
- [Recommendations](#recommendations)
- [Repository Structure](#repository-structure)
- [How to Use This Project](#how-to-use-this-project)
- [Limitations](#limitations)
- [Author](#author)

---

## Project Overview

This project demonstrates a complete workflow from raw data to visual reporting:

1. **Clean and transform** raw order data in Excel.
2. **Validate** the calculated revenue and build **Pivot Tables** for initial insights.
3. **Model and visualise** the cleaned data in Power BI using DAX measures.
4. **Interpret** the results to support sales, pricing and delivery decisions.

**Domain:** E-Commerce / Retail / Sales Analytics

---

## Business Questions

1. Which regions and product categories drive the most revenue, and how does revenue trend over time?
2. How does customer satisfaction (rating) vary by product category, region and payment method?
3. Do longer delivery times reduce customer ratings, and where are delivery delays most visible?
4. How do discounts affect quantity sold and revenue?

---

## Dataset

| Item | Detail |
| --- | --- |
| Source | [Kaggle: E-Commerce Sales Analytics Dataset](https://www.kaggle.com/datasets/abbas829/e-commerce-sales-analytics-dataset) (found via Google Dataset Search) |
| Size used | 500 orders (a sample of the 5,000-row original) |
| Period | 2022 to 2023 |
| Grain | One row per order |

### Columns

| Column | Type | Description |
| --- | --- | --- |
| `order_id` | Text | Unique identifier for each order |
| `order_date` | Date | Date the order was placed |
| `customer_id` | Text | Unique identifier for each customer |
| `product_category` | Categorical | Beauty, Clothing, Electronics, Home |
| `region` | Categorical | East, North, South, West |
| `quantity` | Integer | Units purchased in the order |
| `unit_price` | Decimal | Price of a single unit |
| `discount` | Decimal (%) | Discount applied to the order |
| `payment_method` | Categorical | Card, COD, Wallet |
| `delivery_days` | Integer | Days taken to deliver the order |
| `customer_rating` | Decimal | Customer rating for the order (1 to 5) |
| `revenue` | Decimal | Total revenue earned from the order |

---

## Tools and Technologies

| Tool | Used for |
| --- | --- |
| **Microsoft Excel** | Data cleaning, transformation, calculated fields, Pivot Tables |
| **Power BI Desktop** | Data modelling, DAX measures, interactive dashboard |

---

## Data Pre-Processing (Excel)

The workbook contains three sheets: the original data, the pivot tables, and the cleaned data.

**Cleaning and transformation**

- Removed duplicate orders and handled missing values
- Standardised date and text formats (for example, payment method labels such as `COD` and `Cod`)
- Corrected data types
- Checked for invalid values, such as negative quantities or ratings outside the valid range

**Revenue validation**

The `revenue` column was verified against quantity, unit price and discount:

```
Revenue check      = ROUND(Quantity * Unit Price * (1 - Discount), 2)
Revenue difference = Revenue - Revenue check
```

The largest difference across all 500 orders is 0.01, which is rounding only.

**Pivot Tables built**

1. Revenue by product category
2. Revenue by region
3. Monthly / yearly revenue trend
4. Revenue by payment method
5. Category by region matrix
6. Operations: average delivery days and average rating by region

![Excel Pivot Tables](images/excel-pivot-tables.png)

---

## DAX Measures (Power BI)

```DAX
Total Revenue           = SUM ( 'E_commerce_sales_cleaned _data'[revenue] )
Total Orders            = DISTINCTCOUNT ( 'E_commerce_sales_cleaned _data'[order_id] )
Total Quantity Sold     = SUM ( 'E_commerce_sales_cleaned _data'[quantity] )
Average Customer Rating = AVERAGE ( 'E_commerce_sales_cleaned _data'[customer_rating] )
Average Delivery Days   = AVERAGE ( 'E_commerce_sales_cleaned _data'[delivery_days] )
Average Discount %      = AVERAGE ( 'E_commerce_sales_cleaned _data'[discount] )
```

---

## Dashboard

The report has seven pages: individual chart pages (KPI Cards, Cluster Bar, Line Chart, Donut Chart, Customer Satisfaction, Slicer Applied) and one consolidated **E-commerce Dashboard** page.

**Consolidated page includes**

- **KPI cards:** Total Revenue, Total Orders, Total Quantity Sold, Average Customer Rating
- **Bar chart:** Revenue by product category
- **Line chart:** Monthly revenue trend
- **Bar chart:** Average customer rating by product category
- **Donut chart:** Revenue share by region and payment method
- **Slicers:** Order Date, Region, Product Category, Payment Method

All visuals cross-filter, so selecting a region or category updates every KPI and chart.

---

## Key Findings

| Metric | Result |
| --- | --- |
| Total revenue | **₹502.36K** |
| Total orders | **500** |
| Total quantity sold | **2,025 units** |
| Average order value | ₹1,004.72 |
| Average customer rating | **2.98 / 5** |
| Average delivery time | **6.16 days** |

**Revenue**

- **Electronics** is the top category at ₹175.2K (about 35% of revenue), followed by Clothing (₹136.7K), Home (₹115.9K) and Beauty (₹74.5K).
- **North** is the top region at ₹158.1K (**31.47%** of revenue). The other three regions each contribute roughly 22% to 23%.
- Electronics in the North is the single strongest category-region combination at about ₹74.2K.
- By payment method, **Card** brings in 44.9% of revenue, **COD** 38.3% and **Wallet** 16.9%.

**Customer satisfaction and delivery**

- Ratings are low and flat across the business. Category averages range only from 2.88 (Electronics) to 3.06 (Clothing).
- Electronics earns the most revenue but has the **lowest** average rating.
- **West** has the slowest delivery (6.36 days) and the lowest rating (2.89). **East** has the highest rating (3.03).
- **Wallet** users give the highest average rating (3.15) compared with Card (2.93) and COD (2.95).
- In this sample, delivery days show almost no correlation with rating (r ≈ -0.01), so delivery speed alone does not explain the low scores.

**Discounts**

- The correlation between discount and revenue is weakly negative (r ≈ -0.14), and between discount and quantity it is close to zero (r ≈ -0.06). Higher discounts are not clearly buying higher volumes.

---

## Recommendations

- **Focus marketing and stock** on Electronics and on the North region, which carry the largest share of revenue.
- **Investigate why ratings are low overall**, starting with Electronics and the West region. Delivery speed is a factor to monitor, but the data points to other causes such as product quality, expectations or service.
- **Review discounting.** Limit discounts that do not lift quantity sold or satisfaction.
- **Promote Wallet payments**, since Wallet customers are the most satisfied.
- **Forecast** future revenue from the monthly trend and seasonality, using the Power BI forecast feature on the line chart.

---

## Repository Structure

```
.
├── README.md
├── data/
│   └── E-commerece_Revenue_and_Customer_Satisfaction_Analysis.xlsx
├── dashboard/
│   └── E-commerce_revenue_and_customer_satisfaction.pbix
├── docs/
│   └── Power_BI_Project_Document_E-Commerce.docx
└── images/
    ├── powerbi-dashboard.png
    └── excel-pivot-tables.png
```

| File | Description |
| --- | --- |
| `E-commerece_Revenue_and_Customer_Satisfaction_Analysis.xlsx` | Original data, pivot tables and cleaned data |
| `E-commerce_revenue_and_customer_satisfaction.pbix` | Power BI report and data model |
| `Power_BI_Project_Document_E-Commerce.docx` | Full project documentation |

---

## How to Use This Project

1. **Clone the repository**
   ```bash
   git clone https://github.com/<your-username>/<your-repo-name>.git
   ```
2. **Explore the data:** open the `.xlsx` file in Excel. Start with the `pivot tables` sheet, then see `E_commerce_sales_cleaned _data` for the cleaned dataset.
3. **Open the dashboard:** open the `.pbix` file in [Power BI Desktop](https://powerbi.microsoft.com/desktop/) (free, Windows). If prompted, refresh the data or re-point the source to the Excel file in `data/`.
4. **Read the documentation:** see the Word document in `docs/` for the full write-up.

---

## Limitations

- The analysis uses a **500-row sample**, so findings may not hold for the full 5,000-row dataset.
- The data is not confirmed to be real transactions, so treat the results as an analytical exercise rather than business advice.
- Correlations are simple and do not show causation.

---

## Author

**[Your Name]**
[LinkedIn](https://www.linkedin.com/in/your-profile) | [GitHub](https://github.com/your-username) | your.email@example.com
