# Telecom Customer Churn Analysis

End-to-end analysis of customer churn for a telecom provider, using **Python** for data cleaning, **PostgreSQL** for KPI queries and **Power BI** for an interactive dashboard.

![Customer Churn Analysis Dashboard](images/dashboard.png)

## Business question

Which customers are leaving, why, and which active customers should the retention team focus on first?

## Pipeline

```
Raw CSV (7,043 rows) → Python / Pandas cleaning → PostgreSQL (churn_data) → SQL KPI queries
                                                                         → Power BI dashboard (DAX)
```

## Dataset

7,043 telecom subscribers with 21 attributes: demographics (gender, senior citizen, partner, dependents), account details (tenure, contract, payment method, paperless billing), subscribed services (phone, internet, online security, tech support, streaming), charges (monthly and total) and the target variable **Churn** (Yes/No).

## Data cleaning (Python)

- Standardised column names to lowercase.
- Converted `SeniorCitizen` from 0/1 to No/Yes for readability.
- Converted `TotalCharges` from text to numeric. 11 rows were blank; all had tenure = 0 (new, not yet billed), so they were dropped (0.16% of the data).
- Trimmed whitespace in text columns and merged "No internet service" / "No phone service" into "No".
- Validated the result: no nulls, no duplicate customer IDs, correct data types (7,032 rows).
- Loaded the clean data into PostgreSQL with SQLAlchemy and exported `clean_churn.csv`.

## KPIs (SQL and DAX)

| KPI | Value |
|---|---|
| Total customers | 7,032 |
| Churn rate | 26.58% |
| Total revenue (lifetime charges) | 16.06M |
| Average monthly charges | 64.80 |
| High-risk customers (tenure < 12 months and monthly charges > 70) | 824 |

## Key findings

| Driver | Churn rate |
|---|---|
| Month-to-month contract | 42.7% (vs 11.3% one-year, 2.8% two-year) |
| Tenure of 12 months or less | 47.7% (falls to 6.6% after 5 years) |
| Fiber optic internet | 41.9% (vs 19.0% DSL) |
| Electronic check payment | 45.3% (vs 15–19% other methods) |
| No tech support | 31.2% (vs 15.2% with support) |
| High-risk segment (new and high-paying) | 69% |

Customers who churned paid **74.44** a month on average, compared with **61.31** for those who stayed.

## Recommendations

1. Offer incentives to move month-to-month customers onto one- or two-year contracts.
2. Strengthen onboarding and support during the first 12 months.
3. Bundle tech support or online security for fiber customers.
4. Encourage automatic payment methods over electronic check.
5. Target the active customers in the high-risk segment with retention offers.

## Repository structure

```
data/        Customer_Churn.csv (raw), clean_churn.csv (cleaned)
notebooks/   TelecomCustomerChurn.ipynb (cleaning and load to PostgreSQL)
sql/         CustomerChurnKPIs.sql (table definition and KPI queries)
powerbi/     TelecomCustomerChurnAnalysis.pbix (dashboard)
images/      dashboard.png
```

## How to run

1. Create a PostgreSQL database named `customer_churn`.
2. Run the notebook in `notebooks/`. It asks for your PostgreSQL password when it connects.
3. Create `churn_data` with the script in `sql/`, import `data/clean_churn.csv` into it, then run the KPI queries.
4. Open the `.pbix` file in Power BI Desktop and point the data source at your PostgreSQL server.

## Tools

Python (Pandas, NumPy, SQLAlchemy), PostgreSQL, Power BI (DAX), Jupyter Notebook
