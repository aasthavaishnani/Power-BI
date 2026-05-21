# Power BI ETL Project: Data Leverager

## 1. Project Overview
This project simulates an end-to-end Data Engineering and ETL (Extract, Transform, Load) scenario using Power BI's Power Query Editor. The main objective is to extract data from multiple source types, clean the raw data, perform structural transformations, and create a consolidated data model for business reporting. No visualization or DAX formulas are used, keeping the complete focus on data preparation.

## 2. Data Sources Used
- **Web Data (HTML Table):** Live COVID-19 pandemic statistics by country sourced directly from Wikipedia.
- **Folder Source (Excel Files):** Monthly transaction datasets compiled from individual files (Sales_Jan.xlsx, Sales_Feb.xlsx, Sales_Mar.xlsx, and a simulated Sales_Apr.xlsx).
- **HR Master Data (Excel Workbook):** Employee details containing EmployeeID, Name, Department, Region, and Join Date to establish data relationships.

## 3. Step-by-Step Transformations Applied

### A. Data Extraction & Folder Consolidation
- Connected to the live Wikipedia URL to extract the country-wise pandemic dataset.
- Configured a Folder connection to read monthly sales files simultaneously. 
- Implemented Combine & Transform Data to append individual monthly sheets into a unified data stream.

### B. Data Cleaning & Text Standardization
- Applied Remove Blank Rows and filtered out null fields across key identifiers to keep the data clean.
- Promoted the first row to column headers for consistency.
- Standardized textual data using case transformations (UPPER, LOWER) and removed irregular spacing using TRIM and CLEAN.
- Fixed data types using explicit locales to ensure dates and financial numbers match standard formats.

### C. Numeric Operations & Calculations
- Used Add Column functions to calculate total business metrics.
- Applied the Rounding tool to format all revenue and sales numbers to 2 decimal places.

### D. Time Intelligence & Custom Calendars
- Extracted granular components (Year, Month, Quarter, Day) from the transaction date column.
- Built a custom Fiscal Month calculation using conditional logic to map the financial calendar correctly starting from April.
- Calculated current employee ages from their birthdate field using Duration tools and Round Down operations.

### E. Business Logic & Reshaping
- Created a Conditional Column (Sales Category) to automatically classify transactions into High, Medium, and Low based on specific revenue thresholds (>= 10,000, 5,000–9,999, and < 5,000).
- Added Index Columns (both 0-based and 1-based) to ensure every transaction row contains a unique structural identifier.
- Executed Pivot and Unpivot sequences on time columns to normalize the data structure.

### F. Aggregation & Data Quality Assurance
- Used Duplicate Query to separate the operational data from summary logic.
- Applied Group By operations on regional dimensions to compute Total Sales, Average Order Value, and total Transaction Count.
- Utilized Power Query's Column Quality, Column Distribution, and Column Profile features to verify 100% data integrity with zero errors or empty values.

## 4. Challenges & Solutions

### Challenge 1: Merging Multi-Month Files Automatically
- **Problem:** Manual copy-pasting of monthly files (Jan, Feb, Mar) is slow and errors happen easily when a new month arrives.
- **Solution:** Used the Folder Connect method. When the new Sales_Apr.xlsx file was dropped into the directory, clicking Refresh All successfully pulled the new data into the model without rewriting any logic.

### Challenge 2: Changing Number Column into Date Format
- **Problem:** The date column initially showed as a whole number (123), and changing it directly to a Date data type caused conversion errors due to local system formats.
- **Solution:** Used Change Type with Locale in Power Query, selecting the correct origin format. This resolved all parsing errors instantly.

### Challenge 3: Maintaining Detailed Rows while Creating Summaries
- **Problem:** Applying a Group By operation directly shrinks the entire table, losing individual transactional rows required by the project specifications.
- **Solution:** Created a Duplicate of the primary query named Sales_Summary. Applied all aggregations on this separate table so both the detailed records and regional summaries remain completely intact.

---
*Developed as an Academic ETL Project | Power BI Power Query Assignment*