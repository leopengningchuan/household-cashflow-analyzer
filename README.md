# Household Cashflow Analyzer
Analyze and track household cashflow by consolidating income and expenses

## Table of Contents
- [Project Background](#project-background)
- [Project Goal](#project-goal)
- [File Structure](#file-structure)
- [Instructions](#instructions)
  - [1. Packages Used](#1-packages-used)
  - [2. Statement Data Cleaning](#2-statement-data-cleaning)
  - [3. Transaction Categorization](#3transaction-categorization)
  - [4. Credit Card Validation](#4-credit-card-validation)
  - [5. Transaction Validation](#5-transaction-validation)
  - [6. Mapping Old File Information](#6-mapping-old-file-information)
  - [7. Export Results](#7export-results)
- [Future Improvements](#future-improvements)
- [Acknowledgements](#acknowledgements)
- [License](#license)

## Project Background
Managing household finances often involves scattered statements from multiple accounts, such as checking, savings, and credit cards. These statements typically come in different formats and contain both income and expense records, making it difficult to consolidate and analyze cashflow effectively. Without a structured system, it becomes challenging to monitor spending categories, validate transactions, or gain a clear understanding of overall financial health.

This project addresses that challenge by creating an automated pipeline to clean, unify, and categorize financial statement data. By standardizing inputs from various sources, it provides a consistent foundation for tracking household cash inflows and outflows.

## Project Goal
This project aims to build a **Python Jupyter Notebook** workflow that consolidates and cleans [*Bank of America (BOA)*](https://www.bankofamerica.com/) and [*Discover*](https://www.discover.com/) debit, credit, and savings account statements into a unified dataset. By standardizing transaction records from multiple sources, the tool enables clear analysis of household cash inflows and outflows, supporting accurate tracking of spending, income, and overall cashflow trends.

## File Structure
Configuration & Metadata:
- `README.md` – project overview
- `LICENSE.txt` – license information
- `.gitignore` – git ignore config
- `.gitattributes` – git attributes config

Core Logic:
- `statement_cleaning.ipynb` – notebook for cleaning and unifying the bank statements
- `statement/` – root folder containing all account statements
  - `CC-0401/` – Bank of America credit card (ending 0401)
  - `CC-4253/` – Bank of America credit card (ending 4253)
  - `CC-5257/` – Discover credit card (ending 5257)
  - `DC-8540/` – Bank of America debit card (ending 8540)
  - `DC-9084/` – Bank of America debit card (ending 9084)
  - `SA-7913/` – Bank of America savings account (ending 7913)

## Instructions

### 1. Packages Used
- `pandas`, `datetime`: for data manipulation
- `os`: for file path handling

### 2. Statement Data Cleaning
Transaction records from bank debit, credit, and savings account statements are read from CSV files. Each dataset is cleaned by standardizing column names, parsing dates, and generating unique transaction IDs. Amounts and balances are converted to numeric formats, and account-specific adjustments (e.g., removing flagged rows, validating running balances) are applied to ensure consistency across all sources.

For each debit or saving account, the running balance is used to calculate both the cumulative starting balance (prior to the given date) and the final balance (at the latest available date). After initialization, only transactions on or after the start date are retained to ensure consistency in subsequent analysis.

### 3.Transaction Categorization 
Transaction descriptions are mapped into standardized categories (e.g., Rent, Entertainment, Utility) as column `Type` using a keyword-based mapping. Special cases such as internal transfers between accounts are also normalized to ensure consistency.

### 4. Credit Card Validation
A credit card data validation function is implemented to verify that expense amounts correctly match their corresponding payback amounts. This ensures the integrity of credit card statements before combining them into the unified dataset.

All cleaned and validated datasets are concatenated into one consolidated dataframe, sorted by transaction date for downstream cashflow analysis.

### 5. Transaction Validation
Transaction types are simplified into broader general categories as column `General_Type`. Rows with missing or zero transaction values and categories whose amounts sum to zero are removed are excluded to keep the dataset meaningful. A Validation check is performed to confirm that the sum of the starting balance and all transaction amounts matches the ending balance, guaranteeing financial consistency before moving forward with analysis.

### 6. Mapping Old File Information
The current dataset is merged with a previously saved XLSX file based on transaction IDs, carrying forward existing `Type` and `General_Type` labels where available. By reusing past classifications, it ensures consistency across dataset versions and avoids repeating manual labeling work.

### 7.Export Results
The final cleaned and validated dataset is exported into an XLSX file with a timestamped filename, ensuring that each run generates a uniquely identifiable output for record keeping and future analysis.

## Future Improvements
- **Automated Data Ingestion**: Implement scripts to directly parse PDF or XLSX statements and update the dataset without manual CSV conversion.  
- **Visualization Dashboard**: Build interactive dashboards (e.g., with Power BI or Tableau) to provide clear insights into spending patterns, income trends, and savings progress.  
- **Forecasting Models**: Incorporate time-series forecasting techniques to project future cashflow, helping with budgeting and long-term financial planning.

## Acknowledgements
- Thanks to [*Bank of America (BOA)*](https://www.bankofamerica.com/) for the debit, credit, and savings account statement data used in this project.  
- Thanks to [*Discover*](https://www.discover.com/) for the credit card account statement data used in this project.  

## License
This project is licensed under the MIT License - see the [![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://github.com/leopengningchuan/household-cashflow-analyzer?tab=MIT-1-ov-file) file for details.