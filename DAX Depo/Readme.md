# DAX Depo - Power BI & Business Intelligence Project

## 1. Project Overview
This repository contains the complete implementation of the **DAX Depo** project. The primary objective of this project was to transform raw transactional datasets into a structured and optimized analytical model using Power BI.

The project focuses on:
- Building a clean **Star Schema** data model
- Writing advanced **DAX Calculations**
- Creating reusable **Measures**
- Implementing **Time Intelligence Analysis**
- Designing a professional **Matrix Hierarchy Report** for business decision-making

The final dashboard provides leadership teams with centralized insights into revenue trends, customer behavior, product performance, and regional analytics.

---

# 2. Data Architecture & Star Schema Relationships

To ensure high analytical performance and efficient filter propagation, the Power BI model was designed using a **Star Schema Architecture**.

## Fact Tables
- `Sales_Fact`
- `Returns_Fact`

## Dimension Tables
- `Product_Dim`
- `Customer_Dim`
- `Region_Dim`
- `Date_Dim`

## Relationships

| Fact Table | Relationship | Dimension Table | Key |
|---|---|---|---|
| Sales_Fact | Many-to-One | Product_Dim | ProductID |
| Sales_Fact | Many-to-One | Customer_Dim | CustomerID |
| Sales_Fact | Many-to-One | Region_Dim | RegionID |
| Sales_Fact | Many-to-One | Date_Dim | DateKey |
| Returns_Fact | Many-to-One | Product_Dim | ProductID |
| Returns_Fact | Many-to-One | Date_Dim | DateKey |

---

# 3. Step-by-Step Implementation

# Phase A: Calculated Columns (Row-Level Enhancements)

Several calculated columns were created to improve data readability, categorization, and reporting efficiency.

---

## 1. Return Status Flag (`Sales_Fact`)

Checks whether a sales transaction was returned.

```dax
ReturnFlag = 
IF(
    RELATED(Returns_Fact[ReturnID]) <> BLANK(), 
    "Returned", 
    "Not Returned"
)
```

### Purpose
- Identifies returned transactions
- Enables return-based analysis
- Improves operational tracking

---

## 2. Customer Full Name (`Customer_Dim`)

Combines first and last names into a single formatted column.

```dax
Customer Full Name = 
Customer_Dim[FirstName] & " " & Customer_Dim[LastName]
```

### Purpose
- Simplifies reporting layouts
- Improves readability
- Helps in customer grouping

---

## 3. Sales Performance Categorization (`Sales_Fact`)

Classifies sales transactions into performance bands.

```dax
Sales Category = 
SWITCH(
    TRUE(),
    Sales_Fact[SalesAmount] < 1000, "Low",
    Sales_Fact[SalesAmount] <= 2500, "Medium",
    "High"
)
```

### Purpose
- Creates dynamic sales segmentation
- Supports category-based analysis
- Helps identify transaction patterns

---

## 4. Product Code Generation (`Product_Dim`)

Creates standardized product codes using text manipulation.

```dax
Product Code = 
UPPER(LEFT(Product_Dim[ProductName], 3))
```

### Purpose
- Generates short product identifiers
- Standardizes naming conventions
- Improves report compactness

---

## 5. Month Ending Date (`Date_Dim`)

Maps dates to month-end boundaries.

```dax
Month Ending Date = 
EOMONTH('Date_Dim'[Date], 0)
```

### Purpose
- Supports monthly reporting
- Aligns financial summaries
- Helps in period-based analysis

---

# Phase B: Centralized Measure Repository (`_Measures_Table`)

An empty table named `_Measures_Table` was created to organize all DAX measures in a centralized location.

---

## Core Financial Measures

```dax
Total Sales = 
SUM(Sales_Fact[SalesAmount])

Total Cost = 
SUM(Sales_Fact[Cost])

Total Profit = 
SUM(Sales_Fact[Profit])

Unique Customers Count = 
DISTINCTCOUNT(Sales_Fact[CustomerID])

Average Sale per Transaction = 
AVERAGE(Sales_Fact[SalesAmount])
```

### Purpose
- Tracks financial performance
- Measures profitability
- Analyzes customer activity

---

## Advanced Business & Iterator Metrics

### Return Rate

```dax
Return Rate = 
DIVIDE(
    SUM(Returns_Fact[Quantity]), 
    SUM(Sales_Fact[Quantity]), 
    0
)
```

### Average Profit per Sale

```dax
Average Profit per Sale = 
AVERAGEX(
    Sales_Fact, 
    Sales_Fact[SalesAmount] - Sales_Fact[Cost]
)
```

### Purpose
- Evaluates return behavior
- Measures average profitability
- Uses iterator functions for row-level evaluation

---

## Analytical Filter Context Measures

### Sales Across All Regions

```dax
Sales All Regions = 
CALCULATE(
    [Total Sales], 
    ALL(Region_Dim)
)
```

### Sales for North Region

```dax
Sales North Region = 
CALCULATE(
    [Total Sales], 
    FILTER(
        Region_Dim, 
        Region_Dim[RegionName] = "North"
    )
)
```

### Purpose
- Demonstrates filter context manipulation
- Overrides report filters dynamically
- Enables region-based benchmarking

---

# Phase C: Time Intelligence & Running Calculations

Advanced time intelligence functions were implemented to support trend analysis.

---

## Year-to-Date Sales

```dax
Sales_YTD = 
TOTALYTD(
    [Total Sales], 
    'Date_Dim'[Date]
)
```

---

## Last Year Year-to-Date Sales

```dax
Sales_LY_YTD = 
CALCULATE(
    [Sales_YTD], 
    SAMEPERIODLASTYEAR('Date_Dim'[Date])
)
```

---

## Running Total Sales

```dax
Sales Running Total = 
CALCULATE(
    [Total Sales],
    DATESBETWEEN(
        'Date_Dim'[Date],
        FIRSTDATE(ALL('Date_Dim'[Date])),
        MAX('Date_Dim'[Date])
    )
)
```

---

## Month-over-Month Difference

```dax
Sales MoM Difference = 
[Total Sales] - 
CALCULATE(
    [Total Sales], 
    PREVIOUSMONTH('Date_Dim'[Date])
)
```

### Purpose
- Enables trend analysis
- Tracks cumulative growth
- Compares monthly performance
- Supports executive reporting

---

# 4. Engineering Challenges & Solutions

# The "0.00% YoY Growth" Matrix Issue

## Problem Statement

During report validation, the `Year-Over-Year Sales Growth` measure displayed `0.00%` across all rows in the Matrix Visual, making the dashboard appear incorrect and misleading.

---

## Root Cause Analysis

The dataset only contained records for the calendar year **2023**.

When the following function was executed:

```dax
SAMEPERIODLASTYEAR('Date_Dim'[Date])
```

Power BI attempted to retrieve matching data for **2022**. Since no historical records existed for 2022, the measure:

```dax
[Sales_LY_YTD]
```

returned `BLANK()`.

The original YoY Growth formula attempted to divide by a blank or zero denominator, which caused Power BI to display fallback values of `0.00%`.

---

## Technical Solution

A conditional validation check was implemented to safely handle missing historical data.

```dax
Year-Over-Year Sales Growth = 
IF(
    ISBLANK([Sales_LY_YTD]) || [Sales_LY_YTD] = 0,
    BLANK(),
    DIVIDE(
        [Sales_YTD] - [Sales_LY_YTD],
        [Sales_LY_YTD],
        BLANK()
    )
)
```

---

## Result

The modified logic:
- Eliminated misleading `0.00%` values
- Displayed clean blank outputs when prior-year data was unavailable
- Improved dashboard professionalism and reporting accuracy

---

# 5. Report UI/UX Design Specification

All analytical outputs were consolidated into a single **Matrix Visual** for structured executive reporting.

---

## Row Hierarchy Structure

```text
RegionName
    → MonthName
        → Category
            → Segment
```

### Benefits
- Enables drill-down analysis
- Supports multi-level business exploration
- Improves readability for leadership teams

---

## Value Workspace

The matrix dynamically displays:
- Revenue metrics
- Profitability indicators
- Return percentages
- Time intelligence calculations
- Regional comparisons
- Monthly performance shifts

---

## Interactive Features

### Drill-Down Functionality
Users can:
- Expand regional summaries
- Analyze category-level performance
- Navigate customer segments
- Compare trends across time periods

### Executive Dashboard Design
The layout was optimized for:
- Business leadership reviews
- Fast KPI interpretation
- Clean analytical storytelling
- Structured decision-making workflows

---

# 6. Technologies Used

| Technology | Purpose |
|---|---|
| Power BI | Dashboard Development |
| DAX | Business Calculations |
| Power Query | Data Transformation |
| Star Schema Modeling | Data Architecture |
| Matrix Visuals | Hierarchical Reporting |

---

# 7. Key Learning Outcomes

Through this project, the following concepts were successfully implemented and practiced:

- Advanced DAX Calculations
- Filter Context Manipulation
- Iterator Functions
- Time Intelligence
- Star Schema Design
- Matrix Visual Engineering
- Business KPI Reporting
- Dashboard Optimization
- Data Modeling Best Practices

---

# 8. Conclusion

The **DAX Depo** project demonstrates the practical implementation of modern Business Intelligence workflows using Power BI and DAX.

The project successfully transforms raw operational datasets into a structured analytical reporting environment capable of supporting executive-level decision-making.

The final dashboard combines:
- Strong data modeling practices
- Advanced analytical calculations
- Scalable reporting architecture