# ⚡ Utility Estimate Data Management & Dashboard Analytics App 

![AppSheet Demo GIF] 
(<img width="800" height="449" alt="EstimateAppV1 2-ezgif com-optimize" src="https://github.com/user-attachments/assets/638ccc08-41cb-4b64-91f1-cf552d0f7431" />)


**[▶️ Click here to watch the Full Technical Walkthrough on YouTube](https://www.youtube.com/watch?v=PSWl4bl35bE)**

---

## 📌 Project Overview

**The Data Problem:**
At MPPKVVCL (an electricity distribution company), infrastructure estimate data was siloed across multiple departments. We struggled with messy, unorganized data collected manually through various channels. This lack of a structured data collection process made it incredibly difficult to aggregate real-time metrics and report accurate KPIs to the Ministry of Energy.

**The Analytics Solution:**
I designed an end-to-end data pipeline using Google AppSheet as the data entry point and Google Sheets as a relational backend database. This app acts as a **single source of truth**. By enforcing strict data rules at the point of entry, the system creates a perfectly clean dataset. That data feeds directly into interactive, mobile-friendly dashboards, allowing stakeholders to easily extract data and track project statuses in real time.

---

## 🛠️ Tech Stack & Skills Highlighted
* **Frontend/UI:** Google AppSheet
* **Backend Database:** Google Sheets
* **Core Analytics Skills:** Data Collection Pipelines, Relational Data Modeling, Front-End Data Validation, Data Cleaning, Conditional Logic, Interactive Data Visualization.

---

## 🔐 Data Integrity & Automated Cleaning Logic

To ensure the database receives 100% clean data, I engineered several validation rules to prevent user errors before they happen:

### 1. Primary Keys & Data Validation
* **Automated Indexing:** The system auto-generates serial numbers to serve as unique identifiers (Primary Keys).
* **Data Type Restrictions:** The "Estimate Number" column forces exactly a 6-digit entry. I formatted the database to read this as plain text (e.g., `123456`) instead of a number (`123,456`) to prevent thousands-separator errors during analysis.
* **Error Prevention:** Built-in logic blocks placeholder entries like `0000` and instantly rejects duplicate estimate numbers to prevent dirty data.

### 2. Data Standardization (Dependent Dropdowns)
* **Cascading Filters:** To keep categorical data clean, selecting a specific "Circle" (region) automatically filters the "Division" dropdown to only show relevant options. 
* **Smart Search:** Used a type-ahead search for Applicant Names to prevent spelling variations (e.g., "John Doe" vs "J. Doe"), keeping the dataset standardized. 

### 3. Chronological Rules & Handling Missing Data
* **Automated Formatting:** No matter how a user types a date (DD/MM/YYYY or DD-MM-YYYY), the backend ETL logic automatically transforms and stores it in a uniform format (`DD.MM.YYYY`).
* **Conditional Logic for Nulls:** "Action Date" must be chronologically after the "Inward Date". Additionally, fields like "Reason for Pending" remain hidden unless the status is actually set to "Pending"—preventing conflicting data entries.

---

## 📊 Interactive Dashboards & Visualization

To make this clean dataset actionable for decision-makers, I built two distinct visual dashboards:

* **Analytical Drill-Down Dashboard:** Features a dynamic bar chart. Users can click on a specific region to drill down into the data, filtering the view by status (Approved/Pending) or by specific departments.
* **KPI List Dashboard:** A highly scannable UI list view using conditional formatting. Custom icons appear next to the estimate numbers, providing immediate visual cues on project statuses without needing to query individual records.

---

## 🚀 Business Impact
* Replaced manual, siloed data entry with an automated, structured data collection pipeline accessible via mobile and desktop.
* Drastically improved data accuracy by blocking invalid entries and standardizing formats at the source.
* Saved hours of manual data aggregation by providing management with real-time, self-service dashboard reports.



  ---

## 📸 Application Gallery: Dashboards & Interface
* <img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/df878179-7ed9-4cc6-9031-88dcffb6ecc7" />
* <img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/351918f9-cdac-4c23-9d09-683a931fe7fa" />
* <img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/6ad4ec8d-7898-4958-b13f-b150f2d3c332" />
* <img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/1015999f-5a69-4ca3-946d-7fc5527f3a96" />
* <img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/6c563a63-e26b-4a8c-adfb-828da4fca8d5" />
* <img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/36477529-7029-4300-be20-be0f1c0accf1" />
* <img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/cd773b92-b483-4128-97de-cdd685735846" />
* <img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/482324f2-2d4f-4c17-8ee8-c94f7bdae35f" />
