# Fraud Score Retro Evaluation

## Project Overview
This project evaluates how a fraud scoring model could have been used to prevent bad account outcomes.

Two datasets were analyzed:
- `scores.csv` → contains fraud scores for each application
- `outcomes.csv` → contains the actual outcome (`good_standing`, `fraud`, `charged_off`)

The goal was to determine how different score cutoffs impact both:
- **Fraud prevention**
- **Loss prevention (fraud + charged_off)**

---

## Objectives
- Join score and outcome data
- Evaluate fraud detection performance at different thresholds
- Expand analysis to include broader loss prevention
- Compare precision and recall across multiple cutoffs

---

## Data Preparation
- Loaded both datasets into Pandas DataFrames
- Fixed improperly formatted columns (comma-separated values)
- Converted data types (e.g., `fraud_score` → numeric)
- Joined datasets on `application_id`
- Validated data using `.info()` and `.describe()`

---

## Analysis Approach

### 1. Fraud Prevention
Defined:
- `predicted_fraud = fraud_score >= cutoff`
- `actual_fraud = outcome == "fraud"`

Evaluated performance using:
- Confusion matrix (`pd.crosstab`)
- Precision
- Recall

---

### 2. Loss Prevention (Expanded Definition of Risk)
Defined:
- `predicted_bad = fraud_score >= cutoff`
- `actual_bad = outcome in ["fraud", "charged_off"]`

Evaluated performance using:
- Confusion matrix
- Precision
- Recall

---

## Results Summary

| Use Case          | Cutoff | Precision | Recall |
|------------------|--------|-----------|--------|
| Fraud Prevention | 700    | 0.53      | 1.00   |
| Fraud Prevention | 800    | 1.00      | 1.00   |
| Loss Prevention  | 700    | 1.00      | 1.00   |
| Loss Prevention  | 800    | 1.00      | 0.53   |

---

## Key Insights

- **Fraud @ 700**
  - High recall but lower precision (more aggressive)

- **Fraud @ 800**
  - Perfect precision and recall in this dataset

- **Loss @ 800**
  - Misses a significant portion of bad outcomes (`charged_off`)

- **Loss @ 700**
  - Perfect separation of bad vs clear outcomes *in this dataset*

---

## Business Interpretation

The fraud score appears to segment outcomes into distinct ranges:

- Low scores → `good_standing`
- Mid scores → `charged_off`
- High scores → `fraud`

This suggests a **risk-tier strategy** may be more effective than a single cutoff:

- **< 700 → Approve**
- **700–799 → Review / Monitor**
- **>= 800 → Decline**

---

## Caveats

- Dataset is small (~50 records)
- Results show **perfect separation**, which is unlikely in real-world data
- Performance should be validated on larger and more realistic datasets before production use

---

## Tools Used
- Python
- Pandas
- Jupyter Notebook (VS Code)

---

## Project Structure
fraud_score_retro_eval/
├── data/
├── notebooks/
│   └── fraud_analysis.ipynb
├── README.md
├── requirements.txt
└── main.py

---

## Next Steps

- Test additional cutoffs (e.g., 650, 600)
- Visualize score distributions by outcome
- Evaluate model performance on larger datasets
- Simulate business impact (false positives vs missed losses)