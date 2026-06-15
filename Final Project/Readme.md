````markdown
## Enterprise Sales BI & Return Risk Intelligence Dashboard

**Academic Semester VI Capstone Project**  
**Track:** AI, Machine Learning & Data Science  

---

## 📌 Project Overview

The **Enterprise Sales BI & Return Risk Intelligence Dashboard** is an end-to-end Business Intelligence solution developed using **Power BI Desktop**. The project transforms raw sales data into actionable business insights through data modeling, DAX calculations, and interactive dashboard design.

The dashboard enables executives, marketing teams, and supply chain managers to monitor sales performance, understand customer behavior, and identify product return risks.

---

## 🎯 Objectives

- Analyze enterprise sales performance.
- Identify high-value customers and profitable segments.
- Monitor return trends and risk indicators.
- Support data-driven decision making.
- Deliver a responsive reporting experience across desktop and mobile devices.

---

## 🛠️ Data Engineering & ETL Process

The raw dataset contained missing values, duplicate records, and inconsistent formatting. Power Query was used to clean and transform the data.

### Data Cleaning
- Removed duplicate records.
- Handled null and missing values.
- Standardized customer names.
- Corrected formatting inconsistencies.

### Data Transformation
- Converted fields into optimized data types.
- Structured dates, currency values, and dimensional keys.

---

## 📐 Data Model Architecture

The project follows a **Star Schema** architecture.

### Fact Table

#### Sales_Fact
Stores transactional metrics:

- Revenue
- Profit
- Quantity
- Returns
- Order Date
- Foreign Keys

### Dimension Tables

#### Customer_Dim
- Customer Profiles
- Customer Segments
  - Consumer
  - Corporate
  - Home Office
- Regional Information

#### Product_Dim
- Category
- Sub-Category
- Product Name

#### Date_Dim
- Calendar Dates
- Time Intelligence Support

---

## 🗂️ Calculated Columns

### Date_Dim[YearMonth]

```dax
YearMonth = FORMAT(Date_Dim[Date], "YYYY-MM")
```

Creates a Year-Month hierarchy for trend analysis.

### Customer_Dim[Customer Full Name]

Combines fragmented customer name fields into a single display field.

### Sales_Fact[Is_High_Value_Transaction]

Flags transactions exceeding predefined revenue thresholds.

---

## 🧮 DAX Measures

### Financial Performance Measures

```dax
Total Sales =
SUM(Sales_Fact[Revenue])

Total Profit =
SUM(Sales_Fact[Profit])

Total Orders =
DISTINCTCOUNT(Sales_Fact[OrderID])

Profit Margin % =
DIVIDE([Total Profit], [Total Sales], 0)
```

### Customer Analytics Measures

```dax
Total Active Customers =
DISTINCTCOUNT(Customer_Dim[CustomerID])

Sales Per Customer =
DIVIDE([Total Sales], [Total Active Customers], 0)

Basket Size =
DIVIDE(SUM(Sales_Fact[Quantity]), [Total Orders], 0)

High Value Customer Sales =
CALCULATE(
    [Total Sales],
    FILTER(
        Customer_Dim,
        Customer_Dim[Segment] = "Corporate"
            || Customer_Dim[Segment] = "Home Office"
    )
)
```

### Product & Return Analytics Measures

```dax
Total Quantity Sold =
SUM(Sales_Fact[Quantity])

Total Returns =
SUM(Sales_Fact[Returns])

Return Rate % =
DIVIDE([Total Returns], [Total Orders], 0)

High Margin Sales =
CALCULATE(
    [Total Sales],
    FILTER(
        Product_Dim,
        [Profit Margin %] > 0.25
    )
)
```

---

## 📊 Dashboard Pages

### 🖥️ Page 1 – Executive Summary

High-level business performance monitoring.

#### KPI Cards
- Total Sales
- Total Profit
- Profit Margin %
- Total Orders

#### Visualizations
- Sales & Profit Trend (Line Chart)
- Top 5 Products (Clustered Column Chart)

#### Interactive Features
- Region Slicer
- Reset Filters Button
- Bookmark Navigation

---

### 👥 Page 2 – Customer Intelligence

Customer segmentation and purchasing behavior analysis.

#### KPI Cards
- Sales Per Customer
- Basket Size
- Active Customers
- High Value Customer Sales

#### Visualizations
- Segment Share Treemap
- Customer Performance Matrix

#### Additional Features
- Conditional Formatting
- Data Bars
- Segment Analysis

---

### 📦 Page 3 – Product Analytics & Return Risk

Product performance and return risk analysis.

#### KPI Cards
- Units Sold
- High Margin Sales
- Total Returns
- Return Rate %

#### Visualizations
- Top 10 Products Chart
- Return Risk Scatter Plot

#### Risk Mapping

| Axis | Measure |
|--------|---------|
| X-Axis | Total Sales |
| Y-Axis | Return Rate % |

This helps identify high-revenue products with elevated return risks.

---

## 📱 Mobile Responsive Design

A dedicated Power BI Mobile Layout was designed to support smartphone users.

### Mobile Features

- Single-column responsive layout.
- Mobile-optimized KPI cards.
- Touch-friendly slicers and navigation.
- Enhanced chart readability.
- Improved user experience across devices.

---

## ⚡ Advanced Features

### Custom Tooltips

A dedicated tooltip page displays:

- Product-specific insights.
- Dynamic 12-Month Sales Trend.

### Bookmark Navigation

Implemented using:

- Selection Pane
- Bookmark Pane

Used for:

- Filter Reset
- Navigation Control

### Row-Level Security (RLS)

#### Security Role

```dax
North_Manager
```

#### Security Rule

```dax
[Region] = "North"
```

#### Impact

Users assigned to this role can only access North Region data while all other regional data remains hidden.

---

## ✅ Key Achievements

- Implemented Star Schema modeling.
- Developed advanced DAX calculations.
- Built an interactive multi-page dashboard.
- Designed separate desktop and mobile experiences.
- Configured tooltips, bookmarks, and RLS.
- Delivered actionable business insights.

---

## ⚠️ Limitations

### Current Limitations

- Static Row-Level Security.
- No Dynamic RLS implementation.
- Import Mode architecture.
- No predictive forecasting capabilities.

### Future Enhancements

- Dynamic RLS using USERPRINCIPALNAME().
- Cloud SQL integration.
- DirectQuery implementation.
- AI-powered forecasting.
- Return risk prediction models.

---

## 🚀 Project Execution

1. Open Power BI Desktop.
2. Load the `.pbix` file.
3. Navigate to **Modeling → View As**.
4. Select **North_Manager** to test RLS.
5. Navigate between dashboard pages.
6. Open **View → Mobile Layout** to preview the mobile design.

---

## 🏆 Technologies Used

- Power BI Desktop
- Power Query
- DAX (Data Analysis Expressions)
- Data Modeling
- Star Schema
- Row-Level Security (RLS)
- Bookmarks & Navigation
- Responsive Mobile Layout

---

## 👨‍💻 Academic Learning Outcomes

This project demonstrates practical skills in:

- Business Intelligence
- Data Analytics
- Dashboard Development
- Data Modeling
- Enterprise Reporting
- Decision Support Systems

---

**Developed as part of POWER BI academic coursework in Business Intelligence, Data Analytics, and Enterprise Reporting.**
````
