# Flight Delay Analysis – Root Cause & Impact Dashboard

## 📌 Project Overview
An end-to-end data analysis and business intelligence project analyzing US airline arrival delays (December 2020). This project identifies the primary operational drivers behind flight delays, compares flight delay frequency against total delay duration, and visualizes carrier and airport-level performance.

---

## 🛠️ Tech Stack & Tools
- **Data Modeling & Visualization:** Power BI Desktop, DAX, Power Query
- **Data Processing & EDA:** Python (`pandas`, `numpy`), Matplotlib, Seaborn
- **Architecture:** Star Schema Data Model

---

## 🏗️ Data Model Architecture (Star Schema)
To optimize aggregation performance and prevent data distortion, the dataset was transformed from a denormalized flat structure into a multi-fact **Star Schema**:

- **Dimension Tables:**
  - `Dim_Carrier`: Unique carrier identifiers and airline names (1:Many relationship to Fact tables).
  - `Dim_Airport`: Unique airport codes and names (1:Many relationship to Fact tables).
- **Fact Tables:**
  - `FactDelays`: Flight-level metrics including total flights (`arr_flights`), delayed flight occurrences (`arr_del15`), and total delay duration (`arr_delay`).
  - `FactDelaysCause`: Unpivoted delay causes and corresponding delay minutes to enable category-level filtering.

---

## 🔄 Dashboard Refinement & Iteration (v1 vs. v2)

### Limitations of Version 1 (Flat Model)
- **Granularity Conflicts:** Using a single flat table led to aggregation errors when calculating delay causes alongside flight volumes.
- **Metric Confusion:** Delayed flight count (`arr_del15` = 144) was initially conflated with total delay time (`arr_delay` = 9,060 minutes).
- **Redundant Duplication:** Repeated carrier and airport strings across thousands of rows increased memory overhead and risked double-counting.

### What Was Refined in Version 2
- **Schema Separation:** Decoupled dimensions (`Dim_Carrier`, `Dim_Airport`) from fact measures to ensure strict 1-to-many relationship cardinality.
- **Explicit DAX Calculations:** Replaced implicit visual aggregations with robust DAX measures to isolate count versus duration:
  - `Delayed Flights Count = SUM(FactDelays[arr_del15])`
  - `Total Delay Minutes = SUM(FactDelays[arr_delay])`
  - `Average Delay per Cause = DIVIDE(AVERAGE(FactDelays[arr_del15]), 5, 0)`
- **Visual & UX Clarity:** 
  - Standardized KPI cards to distinctly separate flight volumes, delayed flight counts, and average impact per cause.
  - Formatted Donut Chart labels to display clear percentage shares (`46.6%`, `21.9%`) for readability.
  - Adjusted dimensional axes to prevent category truncation across top-impact airports.

---

## 📊 Key Business Insights
- **Carrier Operations Drive the Majority of Delays:** Carrier-related delays account for **~46.6%** (4.2K minutes) of total delay duration, followed by Late Aircraft delays at **~21.9%** (2.0K minutes).
- **Operational vs. External Causes:** Internal operational inefficiencies (Carrier + Late Aircraft) account for nearly **70%** of all delay time, whereas weather contributes only **~10.5%**.
- **Airport Bottlenecks:** Delays are concentrated heavily in specific regional hubs (e.g., Albuquerque, Allentown), highlighting regional turnaround bottlenecks.

---

## 📁 Repository Structure

├── dataset.csv                   # Raw flight delay dataset
├── eda.ipynb                     # Python exploratory data analysis (EDA)
├── Dashboard.pbix                # Power BI Dashboard file
├── Dashboard_Screenshot.png      # Final dashboard visual screenshot
└── README.md                     # Project documentation

---

## ⚠️ Limitations
- **Time Scope:** The dataset is restricted to December 2020, capturing holiday-season travel patterns but limiting year-round seasonal trend analysis.

---

## 💡 Conclusion & Recommendations
Mitigating airline delays requires targeted operational interventions in aircraft turnaround management and carrier scheduling rather than weather mitigation alone.
