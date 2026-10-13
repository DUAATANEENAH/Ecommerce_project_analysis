# Know Your Customers, Grow Your Sales
## Enterprise Customer & Retail Transaction Analysis Pipeline

[![Python Version](https://img.shields.io/badge/python-3.8%2B-blue.svg)](https://www.python.org/)
[![Status](https://img.shields.io/badge/status-complete-success.svg)]()
[![License](https://img.shields.io/badge/license-MIT-green.svg)]()

---

## Executive Summary
This repository contains a comprehensive data cleaning, feature engineering, and exploratory data analysis pipeline built for an international retail and customer dataset ($541,900+$ raw transactions across 37 countries). 

As businesses scale, raw transaction logs quickly accumulate noise—including unlinked customer accounts, negative return quantities, system adjustments, and administrative fees. Left unchecked, these data anomalies distort executive dashboards and lead to misallocated marketing spend. This project transforms messy transactional logs into a pristine, audit-ready dataset and surfaces actionable customer segmentation insights.

---

## Key Business Questions Answered
1. **Data Trust:** How do we convert unverified raw export files into a trustworthy, standardized database?
2. **Customer Valuation:** Who are our most valuable customers, and what proportion of revenue do they drive?
3. **Geographic Performance:** Which international markets offer the strongest cross-border expansion potential alongside our UK core?
4. **Temporal Trends:** When do peak purchasing surges occur, and how should inventory/marketing budgets adapt?

---

## Project Structure & Processing Workflow

The data processing pipeline executed in the Google Colab notebook adheres to a rigorous senior-level analytics protocol:

1. **Data Ingestion & Audit:**
   * Loaded raw transaction records (`data.csv`).
   * Identified structural anomalies: $135,080$ missing customer IDs, $5,268$ duplicate rows, and multiple negative quantity entries representing order cancellations.
2. **Data Cleaning & Sanitization:**
   * Filtered out unlinked records to ensure accurate customer-level profiling.
   * Removed administrative stock codes (e.g., `BANK CHARGES`, `AMAZONFEE`, `POST`, `DOT`) that represent operational costs rather than retail product sales.
   * Addressed invalid pricing and zero-value test transactions.
3. **Outlier Handling & Comparison:**
   * Evaluated **Removal vs. Capping (Percentile Thresholds)**. Concluded that hard capping distorts legitimate high-volume B2B/wholesale purchases, leading to a selective cleanup strategy focused solely on non-commercial administrative errors.
4. **Categorical Encoding:**
   * Applied **One-Hot Encoding** across the 37 distinct geographic markets (`Country`), preparing features for advanced machine learning models and clustering without imposing false numerical hierarchies.
5. **Feature Engineering:**
   * Generated explicit transactional metrics including `TotalLineRevenue` ($\text{Quantity} \times \text{UnitPrice}$).
   * Extracted temporal components (Year, Month, Day of Week, Hour) from timestamps.
   * Aggregated transactional data into customer-level **RFM (Recency, Frequency, Monetary)** features.

---

## Key Insights & Findings

* **The Top 10% Revenue Rule:** Roughly $10\%$ of unique customer accounts account for over $61\%$ of total revenue. Focusing retention strategies and VIP loyalty tiers on this cohort provides a significantly higher ROI than broad, untargeted campaigns.
* **Core Geographic Concentration:** The United Kingdom accounts for $\approx 82\%$ of total transaction volume, while key secondary markets (such as the Netherlands, EIRE, Germany, and France) represent highly lucrative cross-border opportunities.
* **Q4 Holiday Seasonality:** Sales volume ramps up sharply starting in September and peaks heavily toward the end of the year, providing a clear window for advanced inventory stocking and holiday promotional campaigns.

---

## Getting Started

To run the processing notebook and inspect the pipeline:

1. Clone or download this repository.
2. Open the notebook in Google Colab or your local Jupyter environment.
3. Ensure your raw data file (`data.csv`) is placed in your working directory.
4. Run cells sequentially to execute the data cleaning, feature engineering, and validation checks.

---

## Deliverables
* **Cleaned Dataset:** Audit-ready transaction file free of administrative noise.
* **Interactive Dashboard:** Designed for tracking regional sales and customer lifetime value.
* **Executive Report:** Summarized commercial insights and strategic recommendations.

## Dataset Overview

The raw dataset is the common Online Retail dataset from Kaggle / UCI-style sources. It contains transactions from `01/12/2010` to `09/12/2011` for a UK-based online retailer.

Raw columns:

- `InvoiceNo`: Invoice identifier. Values starting with `C` indicate cancellations.
- `StockCode`: Product code.
- `Description`: Product name.
- `Quantity`: Quantity purchased or returned.
- `InvoiceDate`: Transaction date and time.
- `UnitPrice`: Unit price in sterling.
- `CustomerID`: Customer identifier.
- `Country`: Customer country.

## Project Structure

```text
.
|-- data/
|   |-- raw/
|   |   `-- data.csv
|   `-- processed/
|       |-- online_retail_clean.csv
|       `-- online_retail_clean_ml.csv
|-- notebooks/
|   `-- Ecommerce_analysis.ipynb
|-- reports/
|   |-- client_brief.pdf
|   `-- client_proposal.pdf
|-- requirements.txt
|-- .gitignore
`-- README.MD
```

## Environment Setup

```bash
dowload file from kaggle  https://www.kaggle.com/datasets/carrie1/ecommerce-data/data then add this in data/raw 
```

Create and activate a virtual environment:


```bash
python3 -m venv .venv
source .venv/bin/activate
```

Install dependencies:

```bash
pip install -r requirements.txt
```

Optional: register the environment as a Jupyter kernel:

```bash
python -m ipykernel install --user --name ecommerce-analysis --display-name "Ecommerce Analysis"
```

## How to Run the Project

Start Jupyter:

```bash
jupyter notebook
```

Open and run:

```text
notebooks/Ecommerce_analysis.ipynb
```

The notebook expects the raw file at:

```text
data/raw/data.csv
```

When the notebook is run successfully, it creates or refreshes:

```text
data/processed/online_retail_clean.csv
data/processed/online_retail_clean_ml.csv
```

## Analysis Workflow

1. Data description and initial exploration
2. Missing value, duplicate, and anomaly detection
3. Data cleaning and formatting
4. Feature engineering
5. Business analytics questions
6. Export of cleaned dataset
7. Outlier capping and categorical encoding
8. Export of machine-learning-ready dataset

## Cleaning and Transformation Summary

Main issues found in the raw data:

- `5,268` duplicate rows
- `135,080` missing `CustomerID` values
- `1,454` missing `Description` values
- Negative quantities and prices related to returns, cancellations, or adjustments
- `InvoiceDate` stored as text instead of datetime

Main transformations:

- Removed duplicate rows.
- Filled missing `CustomerID` values with a placeholder for unknown customers.
- Filled or mapped missing product descriptions where possible.
- Created `TransactionType` to separate standard sales from returns.
- Converted `InvoiceDate` to datetime.
- Created `Revenue = Quantity * UnitPrice`.
- Added time-based features: `YearMonth`, `Hour`, and `DayOfWeek`.
- Created ML-ready capped numeric fields.
- Encoded categorical variables such as `TransactionType` and `Country`.

## Output Files

| File | Rows | Columns | Purpose |
| --- | ---: | ---: | --- |
| `data/raw/data.csv` | 541,909 | 8 | Original transaction data |
| `data/processed/online_retail_clean.csv` | 536,641 | 13 | Cleaned analytics dataset |
| `data/processed/online_retail_clean_ml.csv` | 536,641 | 54 | Machine-learning-ready dataset |

## Key Results

From the cleaned dataset:

- Total cleaned records: `536,641`
- Unique invoices: `25,900`
- Unique customers: `4,373`
- Countries represented: `38`
- Total revenue: `9,726,006.95`
- Standard transaction rows: `524,878`
- Return transaction rows: `11,763`
- Final ML dataset missing values: `0`

Top revenue products:

| Rank | Product | Revenue |
| ---: | --- | ---: |
| 1 | DOTCOM POSTAGE | 206,245.48 |
| 2 | REGENCY CAKESTAND 3 TIER | 164,459.49 |
| 3 | PARTY BUNTING | 98,243.88 |
| 4 | WHITE HANGING HEART T-LIGHT HOLDER | 97,659.94 |
| 5 | JUMBO BAG RED RETROSPOT | 92,175.79 |

Top revenue countries:

| Rank | Country | Revenue |
| ---: | --- | ---: |
| 1 | United Kingdom | 8,167,128.18 |
| 2 | Netherlands | 284,661.54 |
| 3 | EIRE | 262,993.38 |
| 4 | Germany | 221,509.47 |
| 5 | France | 197,317.11 |

## Business Questions Supported

The cleaned dataset can be used to answer questions such as:

- What are the monthly sales and revenue trends?
- Which hours and days generate the highest order activity?
- Which products generate the most revenue?
- Which countries are the strongest markets?
- What is the impact of returns on total revenue?
- Which customers have the highest lifetime value?
- How can the cleaned data be used for future segmentation, prediction, or recommendation models?

## Notes

- The raw CSV may require `ISO-8859-1` encoding when loaded directly with pandas.
- The `reports/` folder is reserved for client-facing deliverables.
- Large data files are ignored by Git through `.gitignore`; keep raw and processed datasets locally unless a data storage policy is defined.

## Requirements

Main Python packages:

- pandas
- numpy
- matplotlib
- seaborn
- scikit-learn
- ipykernel
- nbconvert
