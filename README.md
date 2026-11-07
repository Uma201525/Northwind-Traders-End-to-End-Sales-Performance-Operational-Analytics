📊 Northwind E-commerce Sales & Performance Analytics

An end-to-end interactive Power BI dashboard and data analytics solution for monitoring global e-commerce sales performance, supply chain efficiency, customer metrics, and employee achievements using the classic Northwind Traders dataset.

📌 Table of Contents

Project Overview

Key Business Objectives

Data Architecture & Schema

Tools & Technologies Used

Data Pre-Processing

DAX Calculations & Measures

Dashboard Features & Visualizations

Business Insights & Recommendations

How to Run the Project

🏢 Project Overview

The Northwind E-commerce Sales & Performance Analytics Report is designed to provide actionable business insights from historical transactional data (July 2013 – May 2015). By connecting order data, customer behavior, inventory, logistics, and staff management, this project equips decision-makers with real-time tracking of financial metrics, logistics KPIs, and workforce performance.

Key Summary Metrics

Total Sales Revenue: $1.27M

Total Profit: $251.24K

Overall Profit Margin: 19.85%

Sales YTD: $151.40K

YoY Sales Growth: +4.59%

On-Time Delivery Rate: 91.75%

Total Freight Costs: $32.41K

Active Customers: 87

Total Orders: 388

🎯 Key Business Objectives

Monitor Sales & Profitability: Track year-to-date sales and margin percentages across global markets.

Evaluate Product Performance: Identify top-revenue products, high-margin categories, and track discontinued item inventory health.

Optimize Logistics & Delivery: Analyze shipping provider efficiency and control total freight expenditure.

Assess Workforce Performance: Evaluate employee contributions to total revenue and reward top performers.

Analyze Discount Impact: Measure how variations in customer discount percentages affect net revenue and margins.

🗄️ Data Architecture & Schema

The data model connects multiple relational entities into a cohesive Star/Snowflake Schema:

                  ┌──────────────┐
                  │  Categories  │
                  └──────┬───────┘
                         │
┌───────────┐     ┌──────┴───────┐     ┌───────────────┐
│ Customers ├────►│   Products   │◄────┤ Order Details │
└───────────┘     └──────────────┘     └───────┬───────┘
      │                                        │
      │           ┌──────────────┐             │
      └──────────►│    Orders    │◄────────────┘
                  └──────┬───────┘
                         │
            ┌────────────┼────────────┐
            ▼            ▼            ▼
      ┌───────────┐ ┌───────────┐ ┌──────────┐
      │ Employees │ │  Shippers │ │ Calendar │
      └───────────┘ └───────────┘ └──────────┘


Main Entities

Orders & Order Details: Transaction headers and line items (Quantity, Unit Price, Discount, Dates, Freight).

Products & Categories: Product metadata, package sizing, unit pricing, category classifications, and discontinuation status.

Customers & Employees: Client details, employee roles, managerial hierarchy (Reports To), and locations.

Shippers: Shipping carriers and delivery performance metadata.

Calendar: Dedicated time-intelligence mapping table.

🛠️ Tools & Technologies Used

Microsoft Excel / Power Query: Initial data cleaning, structural validation, type casting, and text normalization.

Power BI Desktop: Data modeling, DAX measure creation, time-intelligence calculations, dynamic filters, and visual interface development.

DAX (Data Analysis Expressions): Complex business logic, cumulative totals, YTD tracking, and dynamic calculations.

🧹 Data Pre-Processing

Data Cleaning & Deduplication: Removed duplicate records and handled missing/null values in key tables (Orders, Order Details, Products, Customers, Employees).

Type Standardization: Converted date fields (Order Date, Required Date, Shipped Date) into standard ISO date formats; set currency fields (Unit Price, Freight, Discount) to decimal numeric types.

Text Normalization: Trimmed leading/trailing whitespaces across text fields (Product Name, Company Name, Category Name).

Relational Setup: Configured foreign-key and primary-key relationships with correct cardinality (1-to-many) in Power BI.

📐 DAX Calculations & Measures

Below are key custom DAX measures implemented in the report:

-- 1. Total Revenue
Total_sales = 
SUMX(
    'Order details', 
    'Order details'[Quantity] * 'Order details'[Unit Price] * (1 - 'Order details'[Discount])
)

-- 2. Total Profit
Total_profit = 
SUMX(
    'Order details', 
    ('Order details'[Unit Price] - 10) * 'Order details'[Quantity]
)

-- 3. Profit Margin Percentage
Profit_margin_% = 
DIVIDE([Total_profit], [Total_sales], 0) * 100

-- 4. Year-to-Date (YTD) Sales
Sales_YTD = 
TOTALYTD([Total_sales], 'Orders'[Order Date])

-- 5. Previous Year Sales
Previous_year_sales = 
CALCULATE(
    [Total_sales], 
    SAMEPERIODLASTYEAR('Orders'[Order Date])
)

-- 6. Year-over-Year (YoY) Sales Growth %
Sales YoY Growth % = 
DIVIDE([Total_sales] - [Previous_year_sales], [Previous_year_sales], 0) * 100

-- 7. Cumulative Running Sales Total
Cumulative_Total = 
CALCULATE(
    [Total_sales], 
    FILTER(
        ALL('Orders'[Order Date]), 
        'Orders'[Order Date] <= MAX('Orders'[Order Date])
    )
)


🖼️ Dashboard Features & Visualizations

The executive report provides a unified single-view interface equipped with rich interactive elements:

Executive KPI Cards: Quick overview of Total Sales ($1.27M), Total Profit ($251.24K), YTD Sales ($151.40K), Total Orders (388), Active Customers (87), and Freight Costs ($32.41K).

Sales Trends & Time Intelligence: Line and Area charts showing Sales YTD by Date, temporal breakdown by Year/Quarter/Month, and automated sales forecasting.

Product & Category Analytics: Bar charts and matrix visualizations highlighting top-performing products by revenue vs. profit, along with category-wise inventory health (active vs. discontinued items).

Supply Chain & Freight Dashboard: Gauge charts for On-Time Delivery % (91.75%) and shipper cost breakdown visuals.

Regional & Employee Contributions: Geographical map/donut views displaying market share (USA ~74.4% vs. UK) and employee contribution metrics.

Interactive Controls: Dynamic slicers for Delivery Status (On-Time / Delayed), Country, and Product Discontinued Flag, paired with customized bookmarks for visual navigation.

💡 Business Insights & Recommendations

🔍 Key Diagnostics

Top Categories: Beverages and Dairy Products generate the highest total unit sales volume.

Discount Impact Analysis: High discount rates (Average Discount: 0.06) increase unit sales volumes for specific items but reduce overall profit margins on low-margin products.

Logistics Costs: Higher freight charges from select shipping partners drive overall operational expenses ($1.01M total cost pool).

🚀 Strategic Recommendations

Optimize Freight Contracts: Renegotiate freight agreements with high-cost shipping partners and reallocate delivery volume to cost-effective carriers.

Refine Discount Strategy: Replace broad, flat discounts with targeted discount thresholds to preserve profit margins on lower-margin items.

Customer Retention & Loyalty: Launch tailored retention initiatives for top-tier global accounts to maintain stable order volumes and drive YoY sales growth.
