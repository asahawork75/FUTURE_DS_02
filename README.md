#  Telco Customer Churn Analysis Power BI Dashboard

> **Internship Track:** Data Science & Analytics (`DS`)
> **Repository Name:** `FUTURE_DS_02`
> **Submitted By:** Arindam Saha
> **Submission Date:** 15-06-2026

---

## Table of Contents

1. [Project Overview](#-project-overview)
2. [Dataset Description](#-dataset-description)
3. [Tools & Technologies](#tools--technologies)
4. [DAX Measures & Calculated Columns](#-dax-measures--calculated-columns)
5. [Dashboard Pages](#-dashboard-pages)
6. [Key Insights](#-key-insights)
7. [Business Recommendations](#-business-recommendations)
8. [Repository Structure](#-repository-structure)
9. [How to Use This Project](#-how-to-use-this-project)
10. [Author](#-author)

---

## Project Overview

This project analyzes customer churn for a telecom company using **Power BI Desktop**. The goal is to identify **why customers leave**, **which segments are highest-risk**, **how long customers typically stay**, and **what actions can improve retention** presented as a business-ready dashboard for a SaaS/subscription-style company.

### Business Questions Answered

- Why are customers leaving the platform?
- Which customer segments are most likely to churn?
- How long do customers typically stay active before churning?
- What actions can improve customer retention?

---

## Dataset Description

| Attribute        | Details                                          |
|-------------------|--------------------------------------------------|
| **Source**         | Telco Customer Churn dataset (IBM sample dataset) |
| **Records**        | 7,043 customers                                  |
| **Columns**        | 21 attributes                                    |
| **Target Variable**| `Churn` (Yes/No)                                 |

### Key Columns Used

| Column            | Description                                  |
|--------------------|-----------------------------------------------|
| `customerID`       | Unique customer identifier                    |
| `tenure`           | Number of months the customer has stayed      |
| `Contract`         | Month-to-month / One year / Two year          |
| `InternetService`  | DSL / Fiber optic / No                        |
| `PaymentMethod`    | Electronic check / Mailed check / Bank transfer / Credit card |
| `MonthlyCharges`   | Monthly billing amount (USD)                  |
| `SeniorCitizen`    | Whether the customer is a senior citizen      |
| `Churn`            | Whether the customer left (Yes/No)            |

---

## Tools & Technologies

| Tool                | Purpose                              |
|----------------------|----------------------------------------|
| **Power BI Desktop** | Data modeling, DAX, dashboard design   |
| **DAX**              | Calculated columns & measures          |
| **GitHub**           | Documentation & submission             |

---

## DAX Measures & Calculated Columns

### Calculated Columns

```dax
ChurnFlag =
IF('Telco-Customer-Churn - WA_Fn-UseC_-Telco-Customer-Churn'[Churn] = "Yes", 1, 0)

TenureBucket =
SWITCH(TRUE(),
  'Telco-Customer-Churn - WA_Fn-UseC_-Telco-Customer-Churn'[tenure] <= 6,  "0-6 months",
  'Telco-Customer-Churn - WA_Fn-UseC_-Telco-Customer-Churn'[tenure] <= 12, "7-12 months",
  'Telco-Customer-Churn - WA_Fn-UseC_-Telco-Customer-Churn'[tenure] <= 24, "13-24 months",
  'Telco-Customer-Churn - WA_Fn-UseC_-Telco-Customer-Churn'[tenure] <= 48, "25-48 months",
  "49-72 months"
)

TenureSortOrder =
SWITCH(TRUE(),
  'Telco-Customer-Churn - WA_Fn-UseC_-Telco-Customer-Churn'[TenureBucket] = "0-6 months",   1,
  'Telco-Customer-Churn - WA_Fn-UseC_-Telco-Customer-Churn'[TenureBucket] = "7-12 months",  2,
  'Telco-Customer-Churn - WA_Fn-UseC_-Telco-Customer-Churn'[TenureBucket] = "13-24 months", 3,
  'Telco-Customer-Churn - WA_Fn-UseC_-Telco-Customer-Churn'[TenureBucket] = "25-48 months", 4,
  5
)

RiskTier =
SWITCH(TRUE(),
  'Telco-Customer-Churn - WA_Fn-UseC_-Telco-Customer-Churn'[Contract] = "Month-to-month" &&
  'Telco-Customer-Churn - WA_Fn-UseC_-Telco-Customer-Churn'[InternetService] = "Fiber optic", "High Risk",
  'Telco-Customer-Churn - WA_Fn-UseC_-Telco-Customer-Churn'[Contract] = "Month-to-month", "Medium Risk",
  "Low Risk"
)
```

### Measures

```dax
Total Customers = COUNTROWS('Telco-Customer-Churn - WA_Fn-UseC_-Telco-Customer-Churn')

Churned Customers = SUM('Telco-Customer-Churn - WA_Fn-UseC_-Telco-Customer-Churn'[ChurnFlag])

Churn Rate = DIVIDE([Churned Customers], [Total Customers], 0)

Avg Tenure = AVERAGE('Telco-Customer-Churn - WA_Fn-UseC_-Telco-Customer-Churn'[tenure])

Avg Monthly Charge = AVERAGE('Telco-Customer-Churn - WA_Fn-UseC_-Telco-Customer-Churn'[MonthlyCharges])

Retention Rate = 1 - [Churn Rate]
```

---

## Dashboard Pages

The dashboard contains **4 pages**:

### 1. Executive Summary
- KPI cards: Total Customers, Churn Rate, Avg Tenure, Avg Monthly Charge
- Donut chart: Churned vs Retained customers

### 2. Churn Drivers
- Churn Rate by Contract Type
- Churn Rate by Internet Service
- Churn Rate by Payment Method

### 3. Retention Trends
- Churn Rate by Tenure Bucket (sorted using `TenureSortOrder`)
- Avg Tenure by Contract Type (line chart)

### 4. Customer Segments
- Matrix: Churn Rate by Contract - Internet Service / Payment Method, with conditional formatting
- Churn Rate by Senior Citizen status

---

## Key Insights

| Metric | Value |
|---|---|
| **Overall churn rate** | 26.5% (1,869 of 7,043 customers) |
| **Avg tenure - churned customers** | 18.0 months |
| **Avg tenure - retained customers** | 37.6 months |
| **Avg monthly charge - churned** | $74.44 |
| **Avg monthly charge - retained** | $61.27 |

### Churn Rate by Contract Type
| Contract | Churn Rate |
|---|---|
| Month-to-month | 42.7% |
| One year | 11.3% |
| Two year | 2.8% |

### Churn Rate by Internet Service
| Service | Churn Rate |
|---|---|
| Fiber optic | 41.9% |
| DSL | 19.0% |
| No internet | 7.4% |

### Churn Rate by Payment Method
| Payment Method | Churn Rate |
|---|---|
| Electronic check | 45.3% |
| Mailed check | 19.1% |
| Bank transfer (auto) | 16.7% |
| Credit card (auto) | 15.2% |

### Churn Rate by Tenure Bucket
| Tenure | Churn Rate |
|---|---|
| 0-6 months | 52.9% |
| 7-12 months | 35.9% |
| 13-24 months | 28.7% |
| 25-48 months | 20.4% |
| 49-72 months | 9.5% |

### Churn Rate by Senior Citizen
| Senior Citizen | Churn Rate |
|---|---|
| No | 23.6% |
| Yes | 41.7% |

---

## Business Recommendations

### Why are customers leaving?
Three overlapping factors drive churn: short month-to-month contracts (42.7% churn), high fiber-optic pricing relative to perceived value (41.9% churn), and friction in manual billing - electronic check users churn at 45.3% versus 15-17% for auto-pay users.

### Which segments churn most?
The highest-risk profile is a senior citizen (41.7% churn vs 23.6% for non-seniors) on a month-to-month contract with fiber optic internet, paying via electronic check. A customer matching all of these traits is highly likely to churn within 12 months.

### How long do customers stay?
Churned customers leave after an average of 18 months, while retained customers average 37.6 months. The critical danger window is the first 6 months (52.9% churn rate). Customers who survive past 48 months have only a 9.5% churn risk.

### What actions improve retention?

1. **Push annual contracts** - Offer discounts to convert month-to-month customers to 1-year plans. Moving to a one-year contract drops churn from 42.7% to 11.3%.
2. **Incentivize auto-pay** - Offer a small discount for switching from electronic check (45.3% churn) to credit card or bank transfer auto-pay (15-17% churn).
3. **Build a 90-day onboarding program** - Since 52.9% of 0-6 month customers churn, invest in proactive check-ins, usage support, and a loyalty reward at the 6-month mark.

---

## Repository Structure

```
FUTURE_DS_02_Telco_Churn/
│
├── README.md                          ← Project documentation
├── Telco-Customer-Churn.csv           ← Raw dataset
├── Telco_Churn_Dashboard.pbix         ← Power BI project file
└── images/
    ├── 01_executive_summary.png
    ├── 02_churn_drivers.png
    ├── 03_retention_trends.png
    └── 04_customer_segments.png
```

---

## How to Use This Project

1. **Clone or download this repository**
2. Open `Telco_Churn_Dashboard.pbix` in **Power BI Desktop**
3. If prompted, refresh the data source to point to `Telco-Customer-Churn.csv`
4. Explore the 4 dashboard pages using the page tabs at the bottom
5. Use slicers (Contract, Internet Service, Senior Citizen, Tenure Bucket) to filter and explore segments interactively

---

## Author

| Field         | Details                                  |
|---------------|--------------------------------------------|
| **Name**      | Arindam Saha                            |
| **Track**     | Data Science & Analytics (DS)               |
| **Task**      | Task 02 - Telco Customer Churn Dashboard    |
| **LinkedIn**  | linkedin.com/in/arindam-saha-data-analyst                         |
| **GitHub**    | github.com/asahawork75                |

---

> This project was completed as part of the Future Interns Data Science & Analytics internship programme.
