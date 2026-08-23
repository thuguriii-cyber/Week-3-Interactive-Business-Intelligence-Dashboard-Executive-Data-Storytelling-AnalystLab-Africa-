# DAX Measures Documentation
**AnalystLab Africa — Week 3: Interactive BI Dashboard**
**Dataset:** Telco Customer Churn

This document explains each DAX measure created in the Power BI dashboard: what it calculates, the formula used, and why it matters to the business questions.

---

**Total Customers**
```
Total Customers = COUNTROWS(Telco_Customer_Churn_Cleaned)
```
Counts every customer row in the dataset. Used as the base denominator for churn rate and as a standalone KPI card.

---

**Churned Customers**
```
Churned Customers = CALCULATE(COUNTROWS(Telco_Customer_Churn_Cleaned), Telco_Customer_Churn_Cleaned[Churn] = "Yes")
```
Counts only customers whose Churn value is "Yes." Forms the numerator for Churn Rate.

---

**Active Customers**
```
Active Customers = CALCULATE(COUNTROWS(Telco_Customer_Churn_Cleaned), Telco_Customer_Churn_Cleaned[Churn] = "No")
```
Counts customers who have not churned — the company's current retained customer base.

---

**Churn Rate**
```
Churn Rate = DIVIDE([Churned Customers], [Total Customers], 0)
```
The percentage of customers who have churned. DIVIDE is used instead of the `/` operator so the measure safely returns 0 (instead of an error) if the denominator is ever 0. Directly answers Business Question 1 ("What is the overall customer churn rate?").

---

**Retention Rate**
```
Retention Rate = 1 - [Churn Rate]
```
The inverse of Churn Rate — the percentage of customers the company has successfully retained.

---

**Revenue Lost**
```
Revenue Lost = CALCULATE(SUM(Telco_Customer_Churn_Cleaned[TotalCharges]), Telco_Customer_Churn_Cleaned[Churn] = "Yes")
```
Sums the total historical billing (TotalCharges) of every customer who has since churned — an estimate of revenue no longer being collected due to churn.

---

**Average Tenure**
```
Average Tenure = AVERAGE(Telco_Customer_Churn_Cleaned[tenure])
```
The average number of months customers stay with the company. Used both as a standalone KPI and broken down by Churn status to show how tenure relates to retention.

---

**Average Monthly Charges**
```
Average Monthly Charges = AVERAGE(Telco_Customer_Churn_Cleaned[MonthlyCharges])
```
The average amount customers are billed per month, across the full customer base.

---

**Estimated CLV (Customer Lifetime Value)**
```
Estimated CLV = AVERAGE(Telco_Customer_Churn_Cleaned[MonthlyCharges]) * AVERAGE(Telco_Customer_Churn_Cleaned[tenure])
```
A simplified estimate of the total revenue an average customer generates over their relationship with the company (average monthly spend × average customer lifespan in months). This is a standard simplification for dashboard purposes — it does not factor in acquisition cost, discount rates, or churn probability curves, which a full CLV model would include. This assumption is also noted in the Dashboard Documentation.
