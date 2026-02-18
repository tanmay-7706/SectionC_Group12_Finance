# Financial Transaction Data – Cleaning & Transformation Report

This document outlines the systematic data cleaning, transformation, and feature engineering performed on the Financial Transaction dataset.

The objective of this process was to:

- Ensure data integrity for reliable financial analysis
- Standardize categorical values for consistent reporting
- Handle missing values using business-appropriate methods
- Create KPI-driven calculated metrics
- Prepare structured data for interactive dashboard development

## 1. Data Cleaning Log

| Column Name | Status | Cleaning Reason & Action Taken |
|-------------|--------|--------------------------------|
| Transaction_ID | Cleaned | Missing IDs replaced with "Unknown_ID". Ensured uniqueness. |
| Transaction_Date | Cleaned | Fixed invalid dates (e.g., "Feb 30" → nearest valid date using EOMONTH logic). Standardized to consistent date format. |
| Customer_ID | Cleaned | Missing Customer_IDs replaced with "Unknown_ID". Validated for consistency. |
| Product_Name | Cleaned | Standardized inconsistent variants: "Tab"/"Tabl" → "Tablet"; "Headp" → "Headphones"; "Coffee Ma" → "Coffee Machine"; blank → "Miscellaneous". |
| Quantity | Cleaned | Converted negative values to positive using ABS() (assumed data errors/returns). Removed text contamination. |
| Price | Cleaned | Removed currency symbols and non-numeric characters. Missing prices imputed using **Average Price per Product_Name**. |
| Payment_Method | Cleaned | Standardized variants: "pay pal"/"PayPal " → "PayPal"; "creditcard" → "Credit Card"; other minor variants merged. |
| Transaction_Status | Cleaned | Consolidated free-text statuses into four categories: **Completed**, **Pending**, **Failed**, **Unknown**. |
| Year | Derived | Extracted from Transaction_Date using YEAR() function. |
| Day_of_Week | Derived | Extracted using TEXT(Transaction_Date,"dddd") for Monday-Sunday ordering. |
| Weekend_Flag | Derived | "Weekend" for Sat/Sun, "Weekday" for Mon-Fri. |
| Customer_Segment | Derived | VIP vs Standard based on cumulative revenue threshold per Customer_ID. |
| Revenue_Band | Derived | Bucketed Total_Revenue into Low/Medium/High/Premium bands for analysis. |
| Total_Revenue | Derived | Formula: `Quantity * Price`. |
| Average_Order_Value | Derived | Aggregate metric computed at customer/transaction level for KPI reference. |

## 2. Data Quality Checks: Duplicates, Blanks, and Null Values

A comprehensive data quality audit was performed across all columns.

### Duplicate Detection and Removal

**Process:**
- Checked Transaction_ID and Customer_ID for duplicates
- Identified and removed exact duplicate rows
- Preserved the most complete record for partial duplicates

**Actions Taken:**
- No significant complete duplicates found after ID standardization
- Verified uniqueness of primary keys (Transaction_ID)

### Blank and Null Value Detection

**Process:**
- Systematically scanned every column for blanks, "#N/A", "NULL", empty cells
- Categorized missing values by column type

**Columns Checked:**
- All 15 columns from Transaction_ID to Average_Order_Value

**Correction Strategy:**
- **Numeric Columns (Price, Quantity):** Median or average imputation by product group
- **Categorical Columns:** Mode imputation or logical standardization
- **IDs:** "Unknown_ID" placeholder with documentation
- **Status:** Consolidated into 4 business-relevant categories

**Outcome:**
- Original ~100k rows trimmed to **89,999 clean rows**
- All blank/null cells addressed
- Data completeness achieved across all columns
- Dataset ready for KPI computation and dashboarding

### Data Completeness Summary

| Column Category | Pre-Cleaning Issues | Post-Cleaning Status |
|-----------------|-------------------|---------------------|
| Identifiers | Missing IDs | "Unknown_ID" placeholders |
| Dates | Invalid dates | All valid, standardized format |
| Products/Payments | Inconsistent labels | Standardized categories |
| Numeric (Price/Quantity) | Missing, negative | Imputed, positive only |
| Status | Free-text variants | 4 consistent categories |

## 3. Handling Missing Values Strategy

### Price Imputation Logic

**Why Average by Product?**
- Maintains realistic pricing within each product category
- Better than global median for product-specific analysis
- Preserves relative pricing relationships

**Implementation:**
For each Product_Name: Average_Price = AVERAGEIF(Product_Name_range, Product, Price_range) Missing_Price = Average_Price


### Negative Quantity Handling

**Decision:** Convert to positive using ABS()
- Assumed negative values were data errors or unmodeled returns
- Business context suggests all quantities should be positive counts
- Documented in cleaning log for transparency

## 4. Engineered Features (Calculated Columns)

To support financial analysis and dashboard KPIs:

### A. Total_Revenue

**Definition**
Revenue generated per transaction.

**Formula (Google Sheets)**
=Quantity * Price


**Purpose**
- Core financial metric
- Basis for all revenue-related KPIs
- Enables revenue leakage analysis by status

### B. Customer_Segment (VIP Classification)

**Process**
1. Aggregated revenue by Customer_ID
2. Applied revenue threshold for VIP designation
3. Mapped back to transaction level

**Formula Logic**
IF(Customer_Lifetime_Revenue > threshold, “VIP”, “Standard”)

**Purpose**
- Identifies high-value customers
- Enables segment-specific analysis
- Prioritization for recovery campaigns

### C. Revenue_Band (Order Value Segmentation)

**Formula Used**
=IF(Total_Revenue<20000,“Low”, IF(Total_Revenue<60000,“Medium”, IF(Total_Revenue<120000,“High”,“Premium”)))

**Output Categories**
- Low (< ₹20k)
- Medium (₹20k-₹60k)
- High (₹60k-₹120k)
- Premium (> ₹120k)

**Purpose**
- Revenue concentration analysis
- Failure impact by value band
- Strategic pricing decisions

### D. Weekend_Flag (Temporal Segmentation)

**Formula**
=IF(WEEKDAY(Transaction_Date,2)>5,“Weekend”,“Weekday”)

**Purpose**
- Behavioral pattern analysis
- Capacity planning
- Campaign timing optimization

## 5. Data Aggregations for Dashboard

Pivot table aggregations powering the dashboard:

| Metric | Aggregation Type | Business Insight |
|--------|------------------|------------------|
| Total Revenue | SUM | Revenue realization by status/product/channel |
| Order Count | COUNT | Transaction volume by segment/time |
| Success Rate | COUNTIF / Total | Operational reliability by payment method |
| AOV | SUM/COUNT | Ticket size by customer segment |
| VIP Revenue Share | SUM(VIP)/SUM(Total) | Value concentration risk |
| Weekend Revenue Share | SUM(Weekend)/SUM(Total) | Temporal opportunity identification |

## 6. Dashboard Impact

These transformations enable:

- **KPI Cards** (Revenue, AOV, Success Rate, VIP Share)
- **Trend Charts** (Monthly revenue + success rate)
- **Comparison Charts** (VIP vs Standard, Payment Method performance)
- **Distribution Analysis** (Revenue bands, Day-of-Week patterns)
- **Failure Analysis** (Status by product/channel/day)

## Final Outcome

- Cleaned dataset: **89,999 validated transaction records**
- Standardized categories across all dimensions
- Business-ready calculated metrics (revenue, segments, bands)
- Interactive dashboard foundation established
- Revenue leakage patterns clearly visible and quantifiable

## Project Objective Achieved

The financial transaction dataset is now:

- **Financially accurate** (proper revenue calculation)
- **Operationally reliable** (consistent status tracking)
- **Strategically insightful** (VIP and value band segmentation)
- **Dashboard-ready** (structured for pivots and slicers)
- **Decision-enabling** (clear failure and opportunity identification)
