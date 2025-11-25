# ML-Project-Compilence-Radar

# Compliance Radar – Machine Learning Project

## 1. Introduction

The Compliance Radar project focuses on analyzing organizational compliance behavior using a rich dataset of operational, risk-related, and audit-related variables.  
The goal is to build an analytical foundation that will allow the team to:

- understand compliance patterns,
- prepare the data for machine learning,
- explore structural relationships between variables,
- and later build models that estimate or classify compliance risk.

This repository contains the initial stages of the project, including data loading, cleaning, preprocessing, and exploratory analysis.

---

## 2. Methods (Current Progress)

### 2.1 Data Source

The data is stored in a SQLite database (`org_compliance_data.db`), which contains four tables:

- **departments** — the primary dataset used for analysis.
- **risk_summary_by_division** — supplementary aggregated metrics.
- **high_risk_departments** — external list of flagged departments.
- **data_dictionary** — definitions and descriptions of all variables.

The **departments** table (709 rows) is the main focus of the analysis.

---

### 2.2 Data Loading

The SQLite database was loaded using Python’s `sqlite3` module.  
All tables were inspected, and the `departments` table was selected as the main analytical dataset.

Additional information about feature meanings was taken from the `data_dictionary` table.

---

### 2.3 Data Cleaning

#### **Handling Missing Values**

The dataset contained substantial missingness (up to ~40% in several variables).  
Since dropping rows would result in severe data loss, the following imputation strategy was applied:

- **Median imputation** for numerical columns  
- **Most frequent category (mode)** imputation for categorical columns  
- Identifier fields (`dept_id`, `dept_name`) were left untouched

After imputation, the dataset contained **no missing values**.

---

### 2.4 Outlier Analysis

Outlier detection was performed using the **IQR (Interquartile Range)** method.  
Many variables showed large numbers of outliers due to the nature of compliance and operational behavior (risk exposure, reporting delays, violations, audit results, etc.).

These values represent real departmental behavior, not errors.  
**Therefore, no outlier removal or clipping was applied.**

---

### 2.5 Encoding Categorical Variables

Categorical features such as:

- department type  
- division  
- location type  
- team size  
- reporting structure  
- functional classifications  

were encoded using **one-hot encoding**.  
Identifier fields (`dept_id`, `dept_name`) were excluded from encoding.

The resulting dataset is entirely numeric (except for the two ID fields).

---

### 2.6 Scaling

All numerical variables were standardized using a manual z-score transformation:

\[
z = \frac{x - \text{mean}}{\text{std}}
\]

Scaling was applied to:

- operational metrics  
- risk indicators  
- audit scores  
- reporting metrics  
- binary flags  
- one-hot encoded features  

`dept_id` and `dept_name` were excluded.

This prepares the dataset for models that are sensitive to feature magnitude, such as clustering or logistic regression.

---

## 3. Exploratory Data Analysis (Current Progress)

Initial EDA has been performed, including:

### **3.1 Correlation Analysis**
A correlation matrix and heatmap were generated to evaluate linear relationships among numerical variables.

### **Distribution Analysis**
Histograms were created for all numerical columns to inspect:

- skewness  
- spread  
- multimodality  
- extreme values  

### **Boxplots**
Boxplots were generated for:

- Risk indicators  
- Audit performance metrics  
- Reporting delay and gap metrics  

This helps visualize variability and highlight departments with extreme behavior.

---

## 4. The 3 ML Problems

Based on the structure of the dataset and the results of the exploratory data analysis, the Compliance Radar project involves three complementary ML tasks:

1. Unsupervised Clustering

We use clustering (e.g., K-Means) to identify natural groupings of departments based on operational, audit, and risk-related features.
This helps reveal behavioral patterns and high-risk clusters without predefined labels.

2. Risk Score Prediction (Regression)

The **overall_risk_score** serves as a continuous target variable.
We apply regression models to estimate risk levels from operational metrics, allowing us to quantify the drivers of compliance risk.

3. Feature Importance & Risk Drivers

Tree-based models and correlation analysis are used to determine which factors most influence compliance behavior.
This supports interpretability and contributes directly to the final recommendations.

---


