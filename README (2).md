# Student Performance Analysis & Prediction

Predicts a student's final exam score from information available before the exam, and turns that
prediction into an early-warning flag for students heading towards a fail.

**Author:** Ihsan Mehmood — AI/ML internship task

---

## What this project does

1. Cleans a messy student dataset (duplicates, inconsistent categories, impossible values, missing data).
2. Explores what is associated with performance.
3. Trains and compares two regression models — Linear Regression and Random Forest.
4. Evaluates them with MAE, RMSE and R², against a "predict the average" baseline.
5. Converts the predicted score into an at-risk flag and tunes the cut-off.

## Results

| Model | MAE | RMSE | R² | Cross-validated R² |
|---|---|---|---|---|
| **Linear Regression** (selected) | **6.69** | **8.12** | **0.559** | 0.481 ± 0.091 |
| Random Forest | 7.03 | 8.76 | 0.487 | 0.416 ± 0.055 |
| Baseline — always predict the average | 9.59 | 12.25 | −0.003 | — |

Linear Regression wins on every metric. The relationships in this data are close to linear, so the
extra flexibility of a Random Forest only fits noise.

As an early warning, a cut-off of 60 predicted points catches **21 of the 22** students who scored
below 50, at the cost of 26 false alarms.

## Files

| File | What it is |
|---|---|
| `Student_Performance_Analysis.ipynb` | The notebook — cleaning, analysis, modelling, evaluation |
| `student_performance.csv` | The dataset (662 rows, 13 columns) |
| `make_dataset.py` | The script that generated the dataset |
| `Student_Performance_Report.pdf` | Written report with findings, charts and limitations |
| `README.md` | This file |

## How to run it

### Google Colab (easiest)

1. Go to [colab.research.google.com](https://colab.research.google.com) → **File → Upload notebook**.
2. Upload `Student_Performance_Analysis.ipynb`.
3. Click the folder icon in the left sidebar and upload `student_performance.csv` next to it.
4. **Runtime → Run all**.

### On your own machine

```bash
pip install pandas numpy scikit-learn matplotlib seaborn jupyter
jupyter notebook Student_Performance_Analysis.ipynb
```

Keep `student_performance.csv` in the same folder as the notebook, then run all cells.

To regenerate the dataset from scratch:

```bash
python make_dataset.py
```

The seed is fixed, so it reproduces the same file every time.

## About the dataset

The data is **simulated**, not collected from real students — so it carries no privacy concerns, and
because the true relationships are known they can be checked against what the model learns. The
generator deliberately injects the problems a real school export would have: 12 duplicate rows,
inconsistent spellings (`yes` / `YES` / `Yes`), 6 impossible attendance values of 150%, 4 negative
study-hour values, and 113 missing values across four columns.

**Columns:** `student_id`, `gender`, `study_hours_per_week`, `attendance_percent`, `previous_grade`,
`assignments_submitted`, `absences`, `sleep_hours`, `tutoring`, `internet_access`, `extracurricular`,
`parental_education`, `final_score` (target).

One thing worth knowing if you modify the generator: the parental-education category for "no formal
schooling" must not be written as the string `None` — pandas reads that as a missing value on load.

## Limitations

The data is simulated, so these results show that the pipeline works, not that these effects hold for
real students. The associations found are not causes. Average error is 6.7 points, which is large
enough that predictions near the pass mark flip easily, so the flag should start a conversation and
never decide anything. Gender and parental education are inputs, so error rates would need checking
across those groups before any real use. A reasonable R² is not deployment readiness — there is no
external validation and no monitoring. The full list is in section 9 of the report.

## Built with

Python 3 · pandas · NumPy · scikit-learn · Matplotlib · seaborn
