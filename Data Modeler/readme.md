# Data Modeler – Building a Normalized Star Schema Data Model

## 📌 Project Overview
This repository contains the comprehensive documentation for data modeling, table relationship structuring, and relational schema design for the Power BI project **"Data Modeler"**. 

All data cleaning and structural type-casting were managed within **Power Query**, with structural relationships manually mapped within the **Model View**. To ensure maximum query performance and data integrity, this model relies entirely on natural relationship engine propagation and explicitly avoids calculated columns or row-by-row DAX operations.

---

## 🏗️ Schema Design & Architecture

The analytical model implements a robust hybrid **Star Schema** with an intentional **Snowflake Schema** adjustment to handle multi-fact operational data streams (`Sales_Fact` and `Returns_Fact`) at different analytical grain levels.



### 1. Central Fact Table
* **`Sales_Fact`**: The high-granularity operational core containing numeric keys and measures (`Quantity`, `Revenue`, `Discount`) along with relational transactional foreign keys.

### 2. Dimension Tables (Star Core)
* **`Customer_Dim`**: Contains distinct granular attributes for unique customer records (`CustomerID`, `FullName`, `Age`, `Gender`, `Segment`).
* **`Product_Dim`**: Captures product catalog specifications (`ProductID`, `ProductName`, `Category`, `Subcategory`, `Brand`).
* **`Region_Dim`**: Stores core geographic locations and structural attributes (`RegionID`, `Country`, `State`, `City`).
* **`Date_Dim`**: The foundational enterprise calendar engine dimension (`DateKey`, `Date`, `Month`, `Quarter`, `Year`, `Fiscal Year`).

### 3. Secondary Fact Table (Multi-Fact / Snowflake Extension)
* **`Returns_Fact`**: Captures post-sale transactional return data (`ReturnID`, `SalesID`, `ReturnDateKey`, `Reason`). It establishes a granular dependency on `Sales_Fact` via `SalesID`, forming an extended multi-fact snowflake topology.

---

## 🧩 Cardinality & Relationship Rationale

To prevent data inflation, Cartesian product traps, and redundant scan overheads, all relationships are explicitly constrained according to relational data modeling rules:

| Source Table | Destination Table | Column Fields | Cardinality Type | Cross-Filter Direction | Status |
| :--- | :--- | :--- | :--- | :--- | :--- |
| `Customer_Dim` | `Sales_Fact` | `CustomerID` | `1 : Many (1:*)` | Single (`Customer_Dim` $\rightarrow$ `Sales_Fact`) | **Active** |
| `Product_Dim` | `Sales_Fact` | `ProductID` | `1 : Many (1:*)` | Single (`Product_Dim` $\rightarrow$ `Sales_Fact`) | **Active** |
| `Region_Dim` | `Sales_Fact` | `RegionID` | `1 : Many (1:*)` | Single (`Region_Dim` $\rightarrow$ `Sales_Fact`) | **Active** |
| `Date_Dim` | `Sales_Fact` | `DateKey` | `1 : Many (1:*)` | Single (`Date_Dim` $\rightarrow$ `Sales_Fact`) | **Active** |
| `Sales_Fact` | `Returns_Fact` | `SalesID` | `1 : Many (1:*)` | Single (`Sales_Fact` $\rightarrow$ `Returns_Fact`) | **Active** |
| `Date_Dim` | `Returns_Fact` | `ReturnDateKey` | `1 : Many (1:*)` | Single (`Date_Dim` $\rightarrow$ `Returns_Fact`) | **Inactive** |

### 🔍 Deep Dive: Cardinality Breakdown
* **1:Many ($1:*$) Dimensions to Fact:** Dimension tables use absolute unique Primary Keys (e.g., individual `CustomerID` or `ProductID` records). The `Sales_Fact` table records repeated customer interactions over time, meaning keys map naturally from unique entity rows to multiple transactional records.
* **1:Many ($1:*$) Fact to Fact Lineage:** A single invoice line transaction in `Sales_Fact` (`SalesID`) can technically scale down to multiple item returns or secondary operational events within `Returns_Fact`, creating a structured down-stream $1:*$ mapping.

---

## ⚙️ Advanced Filter Flow & Structural Rules

1. **Strict Unidirectional (Single) Filtering:** Bidirectional filtering is completely disabled across the architecture. Filter context propagates exclusively outward from the isolated master dimensions down to the transaction layers. This mitigates severe engine performance degradation and calculation inflation.
2. **Inactive Relationship Path Propagation:** The connection between `Date_Dim[DateKey]` and `Returns_Fact[ReturnDateKey]` is explicitly set to **Inactive**.
   * *The Modeling Logic:* Enabling this path would generate a structural closed-loop loop (ambiguous multi-path routing) across `Date_Dim`, `Sales_Fact`, and `Returns_Fact`. Power BI enforces model determinism by disabling this line.
   * *Analysis Intent:* To evaluate returns based on actual return dates instead of order booking dates, downstream analytics must invoke this path explicitly using DAX `USERELATIONSHIP()`.

---

## 👥 Model Enhancements & Hierarchies

To streamline business usability and improve visual navigation within reporting views, advanced dimensional optimizations were deployed:

### 1. Advanced Data Categorization
Geographic coordinates are structured with native geospatial metadata attributes to support direct reporting engine mapping visualizations:
* `Region_Dim[Country]` $\rightarrow$ **Country**
* `Region_Dim[State]` $\rightarrow$ **State or Province**
* `Region_Dim[City]` $\rightarrow$ **City**

### 2. Custom Analytical Hierarchies
Three multi-tiered navigation structures were configured to drive granular parent-child drill-downs inside Matrix views without requiring denormalized table merging:
* **Date Hierarchy:** `Year` $\rightarrow$ `Quarter` $\rightarrow$ `Month` $\rightarrow$ `Date`
* **Geographic Hierarchy:** `Country` $\rightarrow$ `State` $\rightarrow$ `City`
* **Product Hierarchy:** `Category` $\rightarrow$ `Subcategory` $\rightarrow$ `ProductName`

---

## ⚠️ Issues Encountered & Engineering Resolutions

### 1. Data Type Mismatch During Relationship Initialization
* **The Problem:** Initial mapping validation between `Sales_Fact[DateKey]` and `Date_Dim[DateKey]` failed, throwing an engine evaluation error indicating that direct relationships could not be formed due to type variance.
* **The Resolution:** Inspected column structures within Power Query and identified that `Sales_Fact` parsed `DateKey` as a raw alphanumeric text string due to source formatting, while `Date_Dim` held it as an integer. Applied explicit schema transformation steps to force both tracking keys to **Whole Number (123)** type, executed row-level whitespace trimming, and applied updates to create a clean $1:*$ link.

### 2. Multi-Path Structural Ambiguity via Returns Table Loop
* **The Problem:** Activating the relationship between `Returns_Fact[ReturnDateKey]` and `Date_Dim[DateKey]` introduced a redundant loop filter path, as the returns model already extracts automated indirect calendar filters via its parent mapping inside `Sales_Fact`.
* **The Resolution:** Adhering strictly to enterprise data warehousing criteria, the secondary calendar linkage was set to **Inactive**. This completely resolved the loop ambiguity, leaving the engine model clean, stable, and deterministic.

---

## ✅ Verification Matrix Results
The structural integrity of table relationships was verified inside a centralized Matrix visualization component based on three distinct corporate requirements:
1. **Product Category & Region Stream:** Proved that filter context distributes correctly across multi-tiered `Product_Dim` hierarchies and `Region_Dim` attributes down into transactional data lakes without generating orphaned rows.
2. **Return Reason Operational Analysis:** Confirmed that `Returns_Fact[Reason]` parses cleanly against `Date_Dim[Fiscal Year]` intervals, illustrating smooth path propagation across cascading boundaries.
3. **Customer Segment Validation:** Validated financial metrics (`Sales_Fact[Revenue]`) broken out across `Customer_Dim[Segment]` keys, indicating accurate dimensional data propagation.