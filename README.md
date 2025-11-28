# Hospital Performance and Financial Dashboard

This repository contains the final Power BI report and source SQL views used for analyzing operational efficiency, provider productivity, and financial risk within the hospital data set.

## 1. Project Overview

The primary goal of this project was to build a multi-page interactive dashboard to provide strategic monitoring for executive leadership and detailed analysis for operational managers.

The report focuses on three key areas:
1.  **Executive Summary:** High-level KPIs and time-based trends.
2.  **Appointments Analysis:** Provider performance benchmarking (Volume vs. Efficiency).
3.  **Financials:** Patient debt, collection status, and risk concentration.

## 2. Connection Notes

This dashboard is powered by data sourced from a **PostgreSQL Database**.

| Component | Detail | Note |
| :--- | :--- | :--- |
| **Data Source Type** | PostgreSQL Database | Connection uses standard port and login. |
| **Connection Method** | Import (via SQL Views) | Data is pulled into the Power BI model. |
| **Gateway Requirement** | **On-premises Data Gateway** | The Gateway is required for scheduled refresh, as the database is not publicly hosted. |
| **Credentials** | **Username/Password** | (No credentials stored here. Managed securely via the Power BI Service Data Gateway.) |

---

## 3. Data Model Diagram

The model adheres to a **Star Schema** to ensure accurate filtering and measure calculation across all fact tables.


<img width="1230" height="496" alt="image" src="https://github.com/user-attachments/assets/410cc5cb-2ebd-4108-8729-d9ce5bf7552a" />

### Key Relationships:

* **Time Filtering:** The **Date** table links to **hospital_appointments_enriched ** via the Date[Date] appointmentdate columns.
* **Financial Filtering:** Financial data is filtered via: **Date**  **hospital_appointments_enriched** **hospital_patients**  **hospital_patient_balances**.
* **Provider Filtering:** **hospital_doctors** links directly to **hospital_appointments_enriched** via DoctorID.

---

## 4. Key Assumptions

The following assumptions were made regarding the source data and modeling:

* **Financial Aggregation:** The **hospital_patient_balances** view provides the current total outstanding balance for each patient. It does not contain historical date information, meaning date filtering for financial KPIs is dependent on the corresponding appointment dates.
* **Time Coverage:** The provided data set contains appointment records only within the year **2021**. Consequently, the **'Appointments YOY Change (%)' KPI is correctly blank**, as no data exists for the prior year (2020) to form a comparison.
* **Data Types:** All necessary data type conversions, including fixing the error in the DateOfBirth column, were performed in Power Query.
* **View Location:** It is assumed that the SQL views detailed in the  /sql/ folder are accessible under the default PostgreSQL schema.

---

## 5. Repository Contents

* **/sql/**: Contains the SQL scripts used to create the pre-aggregated views necessary for the Power BI model.
* **my_hospital_dashboard.pbix**: The final Power BI Desktop file containing the complete data model, measures, and three-page report design.
