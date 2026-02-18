
# Pivot Table Calculations — Method & Working

This document explains how calculations were performed using Pivot Tables and how each metric shown in the dashboard is derived from the cleaned Transaction dataset.

---

# 1. Source Data (Input Fields)

**Transaction_ID**  
Unique identifier for each transaction. Used for counting total transactions and calculating order volume.

**Transaction_Date**  
Cleaned date field used for time-based grouping such as Month and Year.

**Product_Name**  
Standardized product category used for product performance analysis.

**Payment_Method**  
Standardized payment channel used for payment performance and success rate analysis.

**Transaction_Status**  
Transaction outcome category:
- Completed  
- Failed  
- Pending  
- Unknown  

Used to calculate transaction success rate and operational reliability.

**Customer_Segment**  
Customer classification:
- VIP
- Standard

Used to analyze revenue contribution and performance by customer segment.

**Total_Revenue (Calculated Field)**  
Formula:
```
Total_Revenue = Quantity × Price
```

**Day_of_Week (Derived Field)**  
Formula:
```
Day_of_Week = TEXT(Transaction_Date,"dddd")
```

**Revenue_Band (Derived Field)**  
Transactions grouped into categories based on revenue magnitude:
- Low
- Medium
- High
- Premium

---

# 2. Pivot Table Aggregation Logic

## COUNT
Purpose: Measure transaction volume.

Formula:
```
COUNT(Transaction_ID)
```

Used in:
- Total Orders KPI
- Orders by Product
- Orders by Payment Method
- Orders by Customer Segment
- Orders by Revenue Band
- Monthly transaction count

---

## SUM
Purpose: Calculate total revenue performance.

Formula:
```
SUM(Total_Revenue)
```

Used in:
- Monthly Revenue Trend
- Revenue by Product
- Revenue by Customer Segment
- Revenue by Day of Week
- Revenue by Revenue Band
- Overall Total Revenue KPI

---

## Success Rate Calculation
Formula:
```
Transaction Success Rate = Completed Transactions / Total Transactions
```

---

# 3. Pivot Table Structures

## 3.1 Monthly Revenue and Transaction Volume Pivot

Rows:
```
Transaction_Date grouped by Year and Month
```

Values:
```
SUM(Total_Revenue)
COUNT(Transaction_ID)
```

Purpose:
- Identify revenue trends over time
- Detect seasonal patterns
- Measure transaction growth

---

## 3.2 Product-wise Revenue and Transaction Analysis Pivot

Rows:
```
Product_Name
```

Values:
```
SUM(Total_Revenue)
COUNT(Transaction_ID)
```

Purpose:
- Identify best-performing products
- Measure revenue contribution by product
- Analyze product demand

---

## 3.3 Payment Method Transaction Success and Failure Analysis Pivot

Rows:
```
Payment_Method
```

Columns:
```
Transaction_Status
```

Values:
```
COUNT(Transaction_ID)
```

Purpose:
- Measure success rate per payment method
- Identify unreliable payment channels
- Evaluate operational efficiency

---

## 3.4 Customer Segment Performance and Revenue Contribution Pivot

Rows:
```
Customer_Segment
```

Columns:
```
Transaction_Status
```

Values:
```
COUNT(Transaction_ID)
SUM(Total_Revenue)
```

Purpose:
- Compare VIP vs Standard customer performance
- Measure revenue concentration
- Evaluate customer value

---

## 3.5 Day-wise Revenue and Transaction Pattern Analysis Pivot

Rows:
```
Day_of_Week
```

Values:
```
SUM(Total_Revenue)
COUNT(Transaction_ID)
```

Purpose:
- Identify peak business days
- Analyze customer purchase behavior
- Detect weekday vs weekend trends

---

## 3.6 Revenue and Transaction Distribution Across Revenue Bands Pivot

Rows:
```
Revenue_Band
```

Values:
```
SUM(Total_Revenue)
COUNT(Transaction_ID)
```

Purpose:
- Analyze revenue concentration
- Identify high-value transactions
- Understand spending distribution

---

# 4. KPI Card Calculations

## Total Revenue (Completed)
```
SUMIFS(Total_Revenue, Transaction_Status, "Completed")
```

## Total Orders
```
COUNT(Transaction_ID)
```

## Average Order Value
```
SUMIFS(Total_Revenue, Transaction_Status,"Completed")
/
COUNTIFS(Transaction_Status,"Completed")
```

## Transaction Success Rate
```
COUNTIFS(Transaction_Status,"Completed")
/
COUNT(Transaction_ID)
```

## VIP Revenue Share
```
SUMIFS(Total_Revenue, Customer_Segment,"VIP", Transaction_Status,"Completed")
/
SUMIFS(Total_Revenue, Transaction_Status,"Completed")
```

## Weekend Revenue Share
```
SUMIFS(Total_Revenue, Weekend_Flag,"Weekend", Transaction_Status,"Completed")
/
SUMIFS(Total_Revenue, Transaction_Status,"Completed")
```

---

# 5. Dashboard Integration

All Pivot Tables, KPI cards, and charts are connected dynamically.

When filters or slicers are applied:

- Pivot tables update automatically
- Charts refresh automatically
- KPIs recalculate instantly

---

# Summary

All Pivot Table metrics are built on top of the cleaned Working_Data dataset.

Aggregation functions used:

```
COUNT → Transaction volume
SUM → Revenue performance
```

Pivot Tables enable analysis of:

- Revenue trends
- Product performance
- Payment performance
- Customer segmentation
- Revenue distribution
- Customer behavior patterns

All KPIs and dashboards remain dynamic, accurate, and synchronized with the dataset.
