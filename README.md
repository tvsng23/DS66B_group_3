# Forecasting Daily Product Views of an Online Clothing Store from Clickstream Data

Machine Learning course project, National Economics University (NEU), School of Technology.
**Class DS66B – Group 3** · Supervisor: Luong Van Thien

Every evening, a shop manager has to decide which products to feature tomorrow. This project asks:
**how many times will each product be viewed tomorrow, using only information available today?**
We answer it with one-step-ahead forecasting on the public *e-shop clothing 2008* clickstream data
(165,474 clicks, 217 products, April–August 2008).

Before building any model, we re-examine the common `price2` classification task on this dataset and show
experimentally why its high scores do not reflect real predictive ability.

## Contents

- [Key results](#key-results)
- [Repository structure](#repository-structure)
- [Getting started](#getting-started)
- [Notebook parts and report sections](#notebook-parts-and-report-sections)
- [Method in brief](#method-in-brief)
- [Reproducibility](#reproducibility)
- [Limitations](#limitations)
- [Team and contributions](#team-and-contributions)
- [Repository history](#repository-history)
- [Data and references](#data-and-references)

## Key results

| Question | Result |
|---|---|
| Is classifying `price2` meaningful? | No. Accuracy is 100% with `price` (target leakage), 92.91% without `price` under a random split (memorising 217 products), and 44.1% on unseen products, below the 51.2% majority-class rule. |
| Best model (selected on validation) | Random Forest, `max_depth=8` |
| Test MAE / R² (1–12 Aug 2008) | **2.023** views per product per day / **0.711** |
| Strongest baseline | 14-day average, test MAE 2.126, so the Random Forest is **4.8%** better |
| Is the improvement reliable? | Not conclusive: the model beats the baseline on 7 of 12 test days, and a coin-flip simulation reaches that about 39% of the time. |
| What drives accuracy? | Mostly the history features (e.g. the 14-day average), not model complexity. |
| Leakage stress test | Forgetting `shift(1)` inflates R² from 0.711 to 0.786. |
| Classification view ("at least 7 views tomorrow?") | Test F1 0.693 vs 0.682 for a simple rule, so no real gain over regression. |

**Test-set results** (1–12 August 2008, models refitted on train + validation, sorted by test MAE):

| Model | MAE | RMSE | R² |
|---|---|---|---|
| 5. Random Forest (100 trees, `max_depth=8`) | **2.023** | 2.938 | 0.711 |
| 3. KNN (k = 50, history + day of week) | 2.043 | 2.988 | 0.702 |
| 4. Decision Tree (`max_depth=7`) | 2.053 | 2.990 | 0.701 |
| 1b. Linear Regression (all features) | 2.078 | 2.991 | 0.701 |
| 2. Ridge Regression (α = 0.1) | 2.078 | 2.991 | 0.701 |
| 0d. Baseline: 14-day average | 2.126 | 3.065 | 0.686 |
| 0c. Baseline: 7-day average | 2.168 | 3.114 | 0.676 |
| 0b. Baseline: same day last week | 2.627 | 3.779 | 0.522 |
| 1a. Linear Regression (`lag_1` only) | 2.642 | 3.724 | 0.536 |
| 0a. Baseline: yesterday | 2.740 | 3.986 | 0.469 |

See the report for validation results, error analysis, feature importance and the classification view.

## Repository structure

```
DS66B_group_3/
├── README.md
├── requirements.txt
├── .gitignore
├── data/
│   ├── README.md                      # dataset description and references
│   └── e-shop_clothing_2008.csv       # raw dataset (unmodified)
├── notebooks/
│   └── DS66B_group_3.ipynb            # full pipeline, Parts 1–10
├── report/
│   └── Report_EN_DS66B_group_3.pdf    # final report
└── slides/                            # presentation slides
```

## Getting started

Python 3.12 is recommended (the report's results were produced with it).

```bash
git clone https://github.com/tvsng23/DS66B_group_3.git
cd DS66B_group_3

python -m venv .venv
source .venv/bin/activate          # Windows: .venv\Scripts\activate
pip install -r requirements.txt

jupyter notebook notebooks/DS66B_group_3.ipynb
```

Then choose **Run → Run All Cells** (or run the cells from top to bottom).
The notebook reads `../data/e-shop_clothing_2008.csv`, a path relative to the `notebooks/` folder, so launch
Jupyter as above or from inside `notebooks/`.

## Notebook parts and report sections

| Notebook part | Content | Report section |
|---|---|---|
| Part 1 | Loading the data and first checks | 3.1 |
| Part 2 | Data quality checks and cleaning | 3.2 |
| Part 3 | Re-examination of `price2`; choice of problem and algorithms | 3.3, 4.3 |
| Part 4 | Product–day panel, exploratory analysis, feature engineering | 3.4–3.6 |
| Part 5 | One-hot encoding and time-based split | 3.7 |
| Part 6 | Baselines and models, hyperparameters chosen on validation | 3.8, 4.4–4.7 |
| Part 7 | Refit on train + validation, single test evaluation | 3.9, 4.8 |
| Part 8 | Result analysis | 3.10, 4.9–4.12 |
| Part 9 | Classification view | 3.11, 4.13 |
| Part 10 | Conclusion | Conclusion |

Some notebook comments and model labels are in Vietnamese. The report's appendix (Table 5.1) maps every label
to the English name used in the report.

## Method in brief

1. **Clean the data:** remove the truncated last day (13 Aug) and fix the category of product A18.
2. **Build a complete panel:** 217 products × 134 days, with true zeros for days without clicks.
3. **Past-only features** within each product: `lag_1`, `lag_7`, `avg_7_days`, `avg_14_days`, `day_of_week` and
   static product attributes. Every rolling feature is shifted by one day to avoid data leakage.
4. **Time-based split:** Train 15 Apr–15 Jul, Validation 16–31 Jul, Test 1–12 Aug. The test period is used once.
5. **Compare** four naive baselines with Linear Regression, Ridge, KNN, Decision Tree and Random Forest;
   hyperparameters are chosen on the validation period only.
6. **Reliability and error analysis:** day-by-day comparison with the strongest baseline, error by category,
   feature importance and a deliberate "wrong shift" leakage experiment.
7. **Classification view:** Logistic Regression, Naive Bayes, KNN, Decision Tree and Random Forest on the question
   "will a product receive at least 7 views tomorrow?".

## Reproducibility

- Results in the report were obtained by running the notebook from top to bottom in a fresh session with
  Python 3.12, pandas 3.0.2, NumPy 2.4.4, scikit-learn 1.8.0 and Matplotlib 3.10.8
  (pinned in `requirements.txt`).
- All random procedures use `random_state=42` or `np.random.seed(42)`.
- With other library versions, the last decimal of some metrics may change.

## Limitations

- Views are a proxy for demand, not sales; the data contain no purchases.
- The data cover about four and a half months, and the test period lasts 12 days, so the improvement over the
  strongest baseline is promising but not statistically conclusive.
- There is no information on promotions, holidays or campaigns, so sudden traffic spikes cannot be anticipated.
- Display position is fixed for each product, so the models cannot answer causal questions such as the effect of
  moving a product to page 1.

## Team and contributions

| Member | Main tasks | Contribution |
|---|---|---|
| Ho Huyen Trang | Data loading and quality checks (Parts 1–2), re-examination of the `price2` approach (Part 3); Chapters 1 and 2 of the report | 33.33% |
| Ta Van Sang | Product–day table, exploratory analysis and feature engineering (Parts 4–5), leakage checks; Chapter 3 of the report | 33.33% |
| Bui Quang Minh | Models, hyperparameter selection and evaluation (Parts 6–9), classification view; Chapter 4, conclusion and slides | 33.33% |

## Repository history

The project was developed before it was uploaded to GitHub, so the commit history of this repository reflects
the upload and not the development timeline. The notebook was added in three consecutive steps, one for the parts
each member is responsible for (Parts 1–3, Parts 4–5 and Parts 6–10). Later changes to the report, slides and
documentation are committed by the member who made them.

## Data and references

Dataset: *e-shop clothing 2008*, UCI Machine Learning Repository (see [`data/README.md`](data/README.md)).

- M. Lapczynski and S. Bialowas, "Discovering patterns of users' behaviour in an e-shop – comparison of consumer
  buying behaviours in Poland and other European countries", *Studia Ekonomiczne*, no. 151, pp. 144–153, 2013.
- D. Dua and C. Graff, UCI Machine Learning Repository, University of California, Irvine, 2019.

The full reference list is in the report.
