# Telecom Customer Churn Dashboard (Power BI)

An interactive Power BI dashboard analysing why customers leave a telecom company.

![Telecom Customer Churn Dashboard](Telecom%20churn%20dashboard.png)

## Project Overview
This project explores 7,043 telecom customer records to find which customers are most likely to leave (churn), how much revenue is lost, and what the company can do about it.

This is the Power BI version of my [Python churn analysis](https://github.com/Yesiratganiyu/Telecom-customer-churn-analysis), which answers the same questions with pandas and matplotlib.

**Data source:** [Telecom Customer Churn dataset on Kaggle](https://www.kaggle.com/datasets/alipakzad/telecom-customer-churn)

## Tools and Skills
- **Python script in Power BI**: pulled the data directly from Kaggle using the kagglehub library
- **Power Query (M)**: cleaned the data and created new columns
- **DAX**: measures and calculated columns
- **Data visualisation**: KPI cards, donut and bar charts, a slicer and conditional formatting

## Data Preparation (Power Query)
- TotalCharges had 11 empty values. All 11 were brand-new customers (tenure = 0) who had not been billed yet, so the empty values were replaced with 0.
- Created an **AddOnServices** column counting each customer's add-on services (0 to 6), using an M formula with List.Select and List.Count.

## DAX Measures
```dax
Total Customers = COUNTROWS(churn)
Churned Customers = CALCULATE([Total Customers], churn[Churn] = "Yes")
Churn Rate = DIVIDE([Churned Customers], [Total Customers])
Monthly Revenue = SUM(churn[MonthlyCharges])
Revenue Lost = CALCULATE([Monthly Revenue], churn[Churn] = "Yes")
Revenue Lost % = DIVIDE([Revenue Lost], [Monthly Revenue])
Yearly Revenue Lost = [Revenue Lost] * 12
```

A **Tenure Group** calculated column was created with SWITCH(TRUE(), ...) to group customers by how long they have stayed.

## Key Insights
- **26.5%** of customers left, taking **30.5%** of monthly revenue (about 1.67 million a year)
- Month-to-month customers churn at **42.7%**, versus **2.8%** on two-year contracts
- **47.4%** of customers leave in their first year, falling to **9.5%** after 4 years
- Internet customers with no add-ons churn at **52.2%**, while phone-only customers churn at just **7.4%**

## An Interesting Finding
At first, customers with no add-on services seemed less likely to leave (21.4%). Investigating with a slicer showed that this group included phone-only customers, who cannot have internet add-ons and rarely leave. Among internet customers only, those with no add-ons had the highest churn of any group (52.2%).

## Recommendations
1. **Reward longer contracts:** offer a discount for switching to a one-year contract after the first few months
2. **Encourage add-ons:** give new internet customers a free first month of Online Security or Tech Support
3. **First-year welcome programme:** check-in calls and welcome discounts to help customers stay beyond their first year

## Files
- [`Telecom_Churn_Dashboard.pbix`](Telecom_Churn_Dashboard.pbix): the Power BI file (download and open in Power BI Desktop)
- [`churn_dashboard.png`](Telecom%20churn%20dashboard.png): screenshot of the dashboard

**To refresh the data:** You will need to install the Python with `kagglehub` and `pandas`, and Power BI's Python scripting option set to your Python folder because I pulled the data with python from kagglehub.

## Author
**Yesirat Ganiyu**: [LinkedIn](https://www.linkedin.com/in/yesirat-ganiyu/)
