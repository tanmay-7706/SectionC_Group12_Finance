# Financial Transaction Revenue Intelligence Dashboard

**Data Visualization & Analytics Capstone Project**

---

![Dashboard Preview]<img width="737" height="648" alt="Screenshot 2026-02-18 at 11 01 44 PM" src="https://github.com/user-attachments/assets/dcfe77e4-c986-4d33-a4f1-5db7001beb30" />


---

[![Live Dashboard](https://docs.google.com/spreadsheets/d/1cr1qeJf9jVtA1wgLpWgUd_yxEgnW2cgz4zV3d7614Qs/edit?gid=1512876693#gid=1512876693&fvid=1594547284) [!Google Sheets](https://docs.google.com/spreadsheets/d/1cr1qeJf9jVtA1wgLpWgUd_yxEgnW2cgz4zV3d7614Qs/edit?usp=sharing)

---

## Project Overview

This project presents a structured, data-driven analysis of **89,999 cleaned financial transactions** from a raw 100,000-row dataset, built as part of the Data Visualization & Analytics capstone at NST x RU.

The objective was to identify:

- **Revenue leakage** from failed/unknown transactions
- **High-value customer segments** (VIP vs Standard)
- **Payment method reliability** patterns
- **Operational bottlenecks** by time, product, and channel

The final output is an **interactive Revenue Intelligence Dashboard** built in Google Sheets using pivot tables, calculated fields, and dynamic filters.

---

## Problem Statement

Financial operations teams struggle with **messy transaction data** and poor visibility into outcomes, leading to:

- **Silent revenue leakage** — Failed, Pending, and Unknown transactions are not systematically quantified
- **Operational blind spots** — No clear view of which payment methods, products, or days have higher failure rates
- **VIP customer risk** — High-value customers are exposed to the same failure rates as standard customers, with higher financial impact
- **Fragmented reporting** — Manual Excel reports cannot scale or provide self-service analysis

This project addresses these gaps by building a **transaction success and revenue intelligence framework** that translates raw transaction logs into actionable operational and financial insights — helping teams reduce leakage, stabilize channels, and prioritize high-impact fixes.

---

## Business Objective

To design a **KPI-driven analytical framework** that helps:

- Quantify revenue leakage and operational reliability
- Identify VIP customer risk and recovery opportunities
- Optimize payment channel performance
- Enable data-driven operational decisions

---

## Dataset Information

| Attribute | Details |
|---|---|
| **Source** | Kaggle – Synthetic Financial Transactions |
| **Raw Rows** | 100,000 |
| **Cleaned Rows** | 89,999 |
| **Products** | 5 (Tablet, Laptop, Headphones, Coffee Machine, Smartphone) |
| **Payment Methods** | 4+ standardized |
| **Status Categories** | 4 (Completed, Pending, Failed, Unknown) |

**Key Fields Used:** Transaction_ID · Customer_ID · Product_Name · Quantity · Price · Payment_Method · Transaction_Status · Total_Revenue

---

## Data Cleaning & Preparation

All data cleaning was performed entirely in **Google Sheets**.

| Step | Description |
|---|---|
| Product Standardization | "Tab"/"Tabl" → "Tablet"; "Headp" → "Headphones"; blanks → "Miscellaneous" |
| Payment Method Cleaning | "pay pal"/"PayPal " → "Pay Pal"; "creditcard" → "Credit Card" |
| Quantity Fix | Negative values → ABS() (data errors converted to positive) |
| Price Imputation | Missing prices → Average Price per Product_Name |
| Status Standardization | Free-text → 4 consistent categories (Completed/Pending/Failed/Unknown) |
| Date Correction | Invalid dates ("Feb 30") → nearest valid date |

> **Note:** Average price imputation by product preserves pricing realism within categories.

---

## Feature Engineering

Five derived metrics were created to power the dashboard:

**1. Total_Revenue**
Total_Revenue = Quantity * Price

**2. Customer_Segment (VIP Classification)**
IF(Customer_Lifetime_Revenue > threshold, “VIP”, “Standard”)

**3. Revenue_Band**
Low (<₹20K) | Medium (₹20K-₹60K) | High (₹60K-₹120K) | Premium (>₹120K)

**4. Weekend_Flag**
IF(WEEKDAY(Transaction_Date,2)>5, “Weekend”, “Weekday”)

**5. Month_Year**
TEXT(Transaction_Date, “MMM-YYYY”)

---

## KPI Framework

The dashboard tracks the following KPIs, all generated via pivot table aggregations:

| KPI | Formula |
|---|---|
| Total Revenue (Completed) | `SUMIFS(Total_Revenue, Status="Completed")` |
| Average Order Value (AOV) | `Completed_Revenue / Completed_Count` |
| Success Rate % | `Completed_Count / Total_Transactions` |
| VIP Revenue Share % | `VIP_Revenue / Total_Revenue` |
| Weekend Revenue Share % | `Weekend_Revenue / Total_Revenue` |

---

## Key Insights

### Transaction Status Breakdown

| Status | % of Transactions | % of Revenue |
|---|---|---|
| **Completed** | **[fill ~80-90%]** | **100%** |
| Failed | **[fill ~5-10%]** | **0% (lost)** |
| Pending | **[fill ~2-5%]** | **0% (blocked)** |
| Unknown | **[fill ~2-5%]** | **0% (untracked)** |

### Payment Method Reliability

| Method | Success Rate | Revenue Share |
|---|---|---|
| **Credit Card** | **[fill %]** | **[fill %]** |
| **PayPal** | **[fill %]** | **[fill %]** |
| **[Lowest]** | **[fill % — Flag]** | **[fill %]** |

### VIP Customer Impact

- **VIPs contribute [fill ~20-30]% of revenue** despite being a smaller % of transactions
- **VIP failure rates are [fill: similar/higher/lower] than standard**, representing higher financial risk

### Temporal Patterns

- **Weekends show [fill: higher/lower] AOV** despite potentially [fill: higher/lower] volume
- Certain **days of week** exhibit failure spikes, suggesting operational capacity issues

---

## Strategic Recommendations

| Finding | Recommendation | Expected Impact |
|---|---|---|
| [High-failure payment method] | Gateway standardization + monitoring | **[fill %] revenue recovery** |
| VIP revenue concentration | Prioritized failure recovery for VIPs | **High ROI** |
| [Day-specific patterns] | Optimized support staffing | Reduced weekend failures |
| Revenue band concentration | Focus recovery on High/Premium bands | **Maximum revenue impact** |
| Top products identified | Targeted promotions | Increased AOV |

---

## Dashboard Features

The [Google Sheets Dashboard](https://docs.google.com/spreadsheets/d/1cr1qeJf9jVtA1wgLpWgUd_yxEgnW2cgz4zV3d7614Qs/edit?usp=sharing) includes:

- **KPI Cards** (Revenue, AOV, Success Rate, VIP/Weekend Shares)
- **Trend Charts** (Monthly revenue + success rate)
- **Comparison Charts** (VIP vs Standard, Payment Method performance)
- **Distribution Analysis** (Revenue bands, Day-of-Week patterns)
- **Failure Breakdowns** (Status by product/channel/day)
- **Interactive Filters** (Year, Segment, Payment_Method, Status)

> **Google Sheet Name:** Master_Sheet_G12

---

## Limitations

- No customer demographics (age, location) for richer segmentation
- Revenue analysis only (no cost/profit data available)
- Synthetic dataset — patterns are illustrative of methodology
- Static snapshot (no real-time transaction feeds)

---

## Future Improvements

- Predictive failure modeling (ML on payment/time patterns)
- RFM customer segmentation (Recency + Frequency + Monetary)
- Real-time dashboard integration
- Cost modeling for true profitability analysis
- Geographic analysis (if location data available)

---

## Tech Stack

- **Google Sheets** (Pivots, Formulas, Slicers)
- **Data Cleaning & Engineering**
- **Dashboard Design & Layout**
- **Financial KPI Framework**

---

## Conclusion

This project delivers a **transaction intelligence framework** that quantifies revenue leakage, identifies VIP risk, and exposes operational bottlenecks in payment processing.

> **Strongest Insight:** [Fill: e.g., "VIP customers contribute 25% of revenue but face similar failure rates to standard customers, making their recovery the highest ROI operational priority"]

The dashboard provides **self-service analytics** for finance and operations teams, enabling data-driven decisions to stabilize channels, recover lost revenue, and prioritize high-impact fixes.

---

## Quick Links

| Resource | Link |
|---|---|
| Live Dashboard | [Google Sheets – Master_Sheet_G12](https://docs.google.com/spreadsheets/d/1cr1qeJf9jVtA1wgLpWgUd_yxEgnW2cgz4zV3d7614Qs/edit?usp=sharing) |
| Data Cleaning Log | [clean.md](clean.md) |
| Dataset Source | [Kaggle Financial Transactions](https://www.kaggle.com/datasets/alfarisbachmid/dirty-financial-transactions-dataset) |
| Full Project Report | [Project_Report.pdf]([Optimizing Financial Operations [A Data-Driven Approach] (1).pdf](https://github.com/user-attachments/files/25396928/Optimizing.Financial.Operations.A.Data-Driven.Approach.1.pdf)
) |

---

