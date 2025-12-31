# 🏥 Golden Hospital Discharge Record  
### 📊 A Consolidated, Explainable Discharge-Level Dataset

---

## 🧩 Overview

This project implements a **Golden Hospital Discharge Record** by consolidating
multiple hospital source systems into a single, reliable dataset with **exactly one row per discharge (IPID)**.

Healthcare data typically exists across multiple transactional and clinical systems,
often resulting in one-to-many relationships that complicate reporting and analysis.
This solution addresses that challenge by producing a **clean, explainable, and reproducible**
discharge-level dataset suitable for reporting, billing analysis, and downstream analytics.

---

## 🎯 Objective

The primary objectives of this project are:

- 🧱 Build a consolidated discharge-level dataset from multiple hospital systems  
- 🔄 Resolve one-to-many relationships without duplicating discharge records  
- 🧪 Preserve data integrity and analytical explainability  
- ✅ Validate correctness using explicit, auditable checks  

---

## 🗂️ Source Datasets

The following datasets were provided as part of the assessment:

### 1️⃣ `discharge_master.xlsx`
- Primary system-of-record  
- Contains **one row per discharge**  
- Used as the **anchor table** for the golden record  

### 2️⃣ `discharge_details.xlsx`
- Procedure-level billing data  
- Contains **multiple records per discharge (IPID)**  

### 3️⃣ `clinical_findings.xlsx`
- Clinical notes and diagnosis information  
- Contains **multiple records per discharge**  

---

## 🧠 Data Model and Design Decisions

- `discharge_master` treated as the **anchor dataset**  
- Each row in final output represents **exactly one discharge (IPID)**  
- One-to-many datasets were **aggregated prior to joining**  
- Clinical data reduced to **medically relevant summaries**  
- ❌ No artificial imputation or assumptions on missing data  

This design ensures **correctness, transparency, and reproducibility**.

---

## 🔄 Transformations Performed

### 1️⃣ Derived Fields

DISCHARGED_DATE − ADMITTED_DATE (in days)


Provides a standardized inpatient duration metric.

---

### 2️⃣ Procedure Aggregation

From `discharge_details.xlsx`, the following discharge-level metrics were derived:

- 💰 **Total Procedure Amount**  
  `SUM(AMOUNT) per IPID`

- 🔢 **Procedure Count**  
  `COUNT(procedures) per IPID`

These aggregations eliminate row duplication while preserving billing information.

---

### 3️⃣ Clinical Data Handling

From `clinical_findings.xlsx`:

- 👤 Patient demographics (`GENDER`, `AGE`) extracted  
- 🩺 Final diagnosis derived via clinical description filtering  
- ⏱️ Earliest available final diagnosis retained per IPID  
- 📝 Clinical notes concatenated into summarized text  

This balances **clinical relevance** with **analytical usability**.

---

## 🔗 Join Strategy

- All joins performed **after aggregation**  
- Join key: `IPID`  
- Join type: **LEFT JOIN** (preserves all discharges)  
- One-to-one joins enforced to prevent row explosion  

Guarantees **one row per discharge** in the final dataset.

---

## ✅ Validation Checks

To ensure correctness and reproducibility:

- ✔️ Final row count matches `discharge_master`  
- ✔️ Each `IPID` appears **exactly once**  
- ✔️ Aggregated billing totals match raw transactional data  

These checks confirm the **integrity of the golden record**.

---

## 📤 Output

Final outputs generated:

- `outputs/golden_discharge_record.csv`  
- `outputs/golden_discharge_record.xlsx`  

Each row corresponds to **one complete discharge event**.

---

## ▶️ How to Run

### 🧪 Option 1: Notebook Execution

1. Open `notebooks/Golden_Hospital_Discharge_Record_Design.ipynb`  
2. Run all cells sequentially  
3. Outputs generated in `outputs/` directory

---

### ⚙️ Option 2: Script Execution

```bash
python scripts/Golden_Hospital_Discharge_Record_Design.py
```

## 🛠️ Tech Stack Used

- 🐍 **Python 3**
- 📊 **pandas**
- 📄 **openpyxl**
- 📓 **Jupyter Notebook**
- 🧠 **ETL / Data Modeling Concepts**

---

## 📝 Notes

- 📉 No visualizations created — focus is on **data modeling & consolidation**
- 🎯 Design prioritizes **correctness, clarity, and interview explainability**
- 🗄️ Solution is **database-agnostic** and portable to SQL pipelines

---

## 👨‍💻 Author

**Veman S Chippa**  
_Data Analyst / Data Engineer_

🌐 Portfolio: https://iveman.vercel.app/  
💼 LinkedIn: https://www.linkedin.com/in/veman-chippa

