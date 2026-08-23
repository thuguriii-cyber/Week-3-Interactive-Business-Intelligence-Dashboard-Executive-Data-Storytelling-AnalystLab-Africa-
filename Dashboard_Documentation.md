# Dashboard Documentation
**AnalystLab Africa — Week 3: Interactive Business Intelligence Dashboard & Executive Data Storytelling**

## Dashboard Overview
This Power BI dashboard was built for the executive leadership team of a telecom company to monitor customer churn, explore customer behavior, identify business risks, and support retention strategy. It builds on the exploratory analysis completed in Weeks 1-2, using the same Telco Customer Churn dataset.

## Dataset Description
- **Source:** Telco Customer Churn Dataset (Kaggle)
- **Records:** 7,043 customers
- **Fields:** 21 original columns covering demographics (gender, senior citizen, partner, dependents), account information (tenure, contract, payment method, paperless billing), subscribed services (phone, internet, online security, tech support, streaming, etc.), billing (monthly charges, total charges), and churn status.
- **Added field:** `TenureGroup` — customers bucketed into 0-12, 13-24, 25-48, 49-60, and 61+ month ranges, to support tenure-based analysis.

## Data Preparation (Part 1)
- Converted `TotalCharges` from text to a numeric field.
- Set 11 blank `TotalCharges` values (all belonging to customers with 0 months tenure — i.e., not yet billed) to 0, rather than leaving them missing.
- Trimmed stray leading/trailing whitespace across all text columns (notably fixed inconsistent spacing in `PaymentMethod`).
- Confirmed no duplicate customer records.
- Verified correct data types across all numeric, date, and categorical fields.

## Data Model
Single flat table (`Telco_Customer_Churn_Cleaned`) — no additional dimension tables were required given the dataset's structure. All DAX measures are built directly against this table.

## DAX Measures
See **DAX_Measures_Documentation.md** for the full list of 9 measures (Total Customers, Churned Customers, Active Customers, Churn Rate, Retention Rate, Revenue Lost, Average Tenure, Average Monthly Charges, Estimated CLV) with formulas and explanations.

## Dashboard Pages

**1. Executive Overview**
6 KPI cards: Total Customers, Churned Customers, Churn Rate, Retention Rate, Average Monthly Charges, Average Tenure. Includes slicers for Contract, Payment Method, Tenure Group, and Gender, synced across all pages.

**2. Customer Insights**
Visualizes customer demographics and their relationship to churn: churn by Contract type (bar chart), churn by Tenure Group (column chart), Gender split (donut chart), Senior Citizen churn comparison (donut chart).

**3. Service & Churn**
Analyzes churn across subscribed services: a matrix table covering Internet Service, Tech Support, and Online Security churn rates together; a bar chart for churn by Internet Service type; a column chart for churn by Streaming/Multiple Lines.

**4. Revenue**
Analyzes financial impact: revenue by Contract type (column chart), Revenue Lost to churn (KPI card), Average Monthly Charges trend by tenure (line chart), revenue by Payment Method (bar chart).

**5. Customer Detail (Drill-through page)**
A detail table (Customer ID, Tenure, Monthly Charges, Churn) that filters to a specific Contract type when a user drills through from any other page.

## Interactivity
- **Slicers:** Contract, Payment Method, Tenure Group, Gender — synced across all report pages.
- **Navigation buttons:** page-to-page navigation buttons placed on each page.
- **Bookmarks:** "Default View" (no filters applied) and "Churned Customers Only" (Churn = Yes filter applied), for quick executive access to common views.
- **Drill-through:** Contract field enabled as a drill-through target, landing on the Customer Detail page.

## Business Assumptions
- Estimated CLV is calculated as average monthly charge × average tenure — a simplified approach that does not account for acquisition cost, discount rates, or churn probability curves.
- Customers with 0 tenure and blank TotalCharges are treated as newly joined, not yet billed, and their TotalCharges is set to 0 rather than excluded from the dataset.

## Limitations
- The dataset represents a single snapshot in time rather than a time series, so trend analysis (e.g., month-over-month churn) is not possible with this data alone.
- CLV is an estimate, not a precise financial calculation.
- The dataset does not include customer satisfaction scores, support ticket history, or competitor pricing, all of which could add further explanatory power to the churn drivers identified.
