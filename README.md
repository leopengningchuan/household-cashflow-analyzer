# Household Cashflow Analyzer
Analyze and track household cashflow by consolidating income and expenses

## Table of Contents
- [Project Background](#project-background)
- [Project Goal](#project-goal)
- [File Structure](#file-structure)
- [Instructions](#instructions)
  - [1. Packages Used](#1-packages-used)
  - [2. Datasets Used](#2-datasets-used)
  - [3. Statement Data Cleaning](#3-statement-data-cleaning)
  - [4. Transaction Categorization](#4-transaction-categorization)
  - [5. Transaction Offset](#5-transaction-offset)
  - [6. Mapping Old File Information](#6-mapping-old-file-information)
  - [7. Export Results](#7-export-results)
  - [8. Online Dashboard](#8-online-dashboard)
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
- `.gitmodules` – git submodules config

Core Logic:
- [`utils/`](https://github.com/leopengningchuan/personal_utils) – submodule used
- `statement_cleaning.ipynb` – notebook for cleaning and unifying the bank statements
- [`Household Cashflow Analyzer`](https://public.tableau.com/app/profile/leo.peng.ningchuan/viz/HouseholdCashflowAnalyzer/sy) – a dashboard based on the generated data in Tableau Public

## Instructions

### 1. Packages Used
- `pandas`, `datetime`: for data manipulation
- `os`: for file path handling
- `dotenv`: for loading environment variables from a `.env` file
- [`personal_utils.google_api_utils`](https://github.com/leopengningchuan/personal_utils): for updating dataset to Google Sheet

### 2. Datasets Used
This project utilizes the bank account statements in CSV format from [*Bank of America (BOA)*](https://www.bankofamerica.com/) and [*Discover*](https://www.discover.com/).

Due to privacy, the raw dataset are not shared in this repository.

### 3. Statement Data Cleaning
Transaction records from bank debit, credit, and savings account statements are ingested and standardized by harmonizing column names, parsing dates, and generating unique transaction IDs. Column values are converted into consistent and meaningful formats. After initialization, only transactions occurring on or after the specified start date are retained to ensure consistency in subsequent analysis. Finally, all datasets are consolidated into a single unified dataset.

### 4. Transaction Categorization
Transaction descriptions are mapped into standardized categories (e.g., Rent, Entertainment, Utility) as column `Type` using a keyword-based mapping. Special cases such as internal transfers between accounts are also normalized to ensure consistency.

### 5. Transaction Offset
To maintain data quality, rows with missing or zero transaction amounts are excluded, and categories whose aggregated amounts sum to zero are removed.

### 6. Mapping Old File Information
The current dataset is merged with a previously saved XLSX file based on transaction IDs, carrying forward existing `Amount`, `Type` and `General_Type` labels where available. By reusing past classifications, it ensures consistency across dataset versions and avoids repeating manual labeling work. Transaction types are simplified into broader general categories as column `General_Type`.

In addition, the script checks that rows with `Type = 'Omit: others'` sum to 0, and that `Original_Amount` and `Amount` totals match. If either check fails, a `ValueError` is raised.

### 7. Export Results
The final cleaned and validated dataset is exported into an XLSX file with a timestamped filename, ensuring that each run generates a uniquely identifiable output for record keeping and future analysis.

The dataset is also published to a Google Sheet, using `gsheet_upload()` from the `utils.google_api_utils` module to ensure that Tableau Public reflects the latest data upon refresh for further analysis.

The `utils/` folder is included as a Git submodule and contains a reusable function library maintained in the [`personal_utils`](https://github.com/leopengningchuan/personal_utils). You can refer to that repository for detailed function documentation and personal notes.

### 8. Online Dashboard
The cleaned dataset powers an interactive Tableau Public dashboard of household cashflow. Analysis can be conducted through date ranges, income/expense type, and spender to reveal monthly trends and breakdowns.

Please view the dashboard here: [*Household Cashflow Analyzer*](https://public.tableau.com/app/profile/leo.peng.ningchuan/viz/HouseholdCashflowAnalyzer/sy)

## Future Improvements
- **Automated Data Ingestion**: Implement scripts to directly parse PDF or XLSX statements and update the dataset without manual CSV conversion.
- **Visualization Dashboard**: Build interactive dashboards (e.g., with Power BI or Tableau) to provide clear insights into spending patterns, income trends, and savings progress.
- **Forecasting Models**: Incorporate time-series forecasting techniques to project future cashflow, helping with budgeting and long-term financial planning.

## Acknowledgements
- Thanks to [*Bank of America (BOA)*](https://www.bankofamerica.com/) for the debit, credit, and savings account statement data used in this project.
- Thanks to [*Discover*](https://www.discover.com/) for the credit card account statement data used in this project.
- Thanks to [*Tableau Public*](https://public.tableau.com) for providing a online platform to share dashboards.

## License
This project is licensed under the MIT License - see the [![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://github.com/leopengningchuan/household-cashflow-analyzer?tab=MIT-1-ov-file) file for details.
