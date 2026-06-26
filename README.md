# Electric Vehicle Population Data Cleaning and Transformation (SQL)

## Project Overview
This project focuses entirely on data engineering, data quality auditing, and structural transformation using SQL. The dataset contains comprehensive records of Electric Vehicles (EVs) and Plug-in Hybrid Electric Vehicles (PHEVs), tracking technical specifications, geographic distribution, and logistical indicators.

The goal of this project was to take an unpolished production dataset and apply rigorous data cleaning frameworks to guarantee data integrity before it ever hits an enterprise data warehouse or business intelligence layer.

* **Dataset Source:** [Washington State Department of Licensing (Open Data)](https://catalog.data.gov/dataset/electric-vehicle-population-data)

---

## Data Cleaning and Transformation Framework

The project was executed across four structured operational stages to isolate raw data from the transformation engine and systematically fix quality issues.

### Stage 1: Architectural Isolation (Staging)
To adhere to standard data warehouse safety principles, the original data layer was kept entirely pristine and untouched. A dedicated, decoupled staging table was created to run all structural changes:
* Created `vehicle_population_staging` as an independent working layer.

### Stage 2: Comprehensive Duplication Auditing
A high-integrity deduplication audit was executed across all core transactional and machine-level attributes (including VINs, Department of Licensing IDs, and exact geographic markers):
* Applied an advanced window function using `ROW_NUMBER() OVER (PARTITION BY ...)` to flag matching records across the entire composite primary column key.
* Verified that the dataset maintained zero duplicate rows, confirming transaction-level uniqueness.

### Stage 3: Data Standardization and Normalization
Text fields and categorical attributes were cleaned to eliminate typos, casing errors, and regional inconsistencies that break downstream analytical models:
* Handled structural text formatting across state names and county attributes.
* Standardized state regional mappings using conditional updates (e.g., explicitly ensuring standard text compliance for geographic attributes across Washington, California, and out-of-state entities).

### Stage 4: Strategic Null and Missing Value Imputation
Instead of dropping rows with missing values—which would destroy aggregate volume counts and skew global analytics—empty fields and `NULL` values were handled with custom business logic:
* Imputed missing geographic attributes, cities, and postal codes to a standardized `'Unknown'` flag.
* Handled missing legislative districts by standardizing them into an appropriate operational placeholder.
* Standardized missing or unlinked utility providers to `'Non-WA'` or general defaults to preserve regional operational metrics.

---

## Core SQL Skills Demonstrated
* Data Architecture Safety: Isolating raw operational layers from the active transformation environment using staging engines.
* Quality Assurance Auditing: Writing advanced window functions (`ROW_NUMBER`) to check database rows for duplicate anomalies.
* Conditional Logic Mutations: Utilizing `CASE WHEN` statements, string operations, and independent column updates to normalize data fields.
* Missing Value Management: Applying column modifications and precise data imputation to maintain total row volume while removing data noise.
