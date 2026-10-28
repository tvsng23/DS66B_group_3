# Forecasting Daily Product Views of an Online Clothing Store from Clickstream Data

Machine Learning course project, National Economics University (NEU), School of Technology.
**Class DS66B – Group 3** · Supervisor: Luong Van Thien

We forecast how many times each product of an online clothing store will be viewed **tomorrow**,
using only information available today. Before that, we re-examine the common `price2`
classification task on this dataset and show why its high scores are not meaningful.

## Key results

| Question | Result |
|---|---|
| Is classifying `price2` meaningful? | No. Accuracy is 100% with `price` (target leakage), 92.91% without `price` under a random split (memorising 217 products), and 44.1% on unseen products, below the 51.2% majority-class rule. |
| Best model (selected on validation) | Random Forest, `max_depth=8` |
| Test MAE / R² (1–12 Aug 2008) | **2.023** views per product per day / **0.711** |
| Strongest baseline | 14-day average, test MAE 2.126, so the Random Forest is **4.8%** better |
| Is the improvement reliable? | Not conclusive: the model wins on 7 of 12 test days, and a coin-flip simulation reaches that about 39% of the time. |
| Leakage stress test | Forgetting `shift(1)` inflates R² from 0.711 to 0.786. |
| Classification view ("≥ 7 views tomorrow?") | Test F1 0.693 vs 0.682 for a simple rule, so no real gain over regression. |

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

## How to run

Python 3.12 is recommended (the report's results were produced with it).

```bash
git clone https://github.com/<owner>/DS66B_group_3.git
cd DS66B_group_3

python -m venv .venv
source .venv/bin/activate          # Windows: .venv\Scripts\activate
pip install -r requirements.txt

jupyter notebook notebooks/DS66B_group_3.ipynb
```

Run all cells from top to bottom. The notebook reads `../data/e-shop_clothing_2008.csv`
(path relative to the `notebooks/` folder), so start Jupyter as above or from inside `notebooks/`.
All random procedures use `random_state=42` or `np.random.seed(42)`. With other library versions
the last decimal of some metrics may change.

## Notebook parts and report sections

| Notebook part | Content | Report section |
|---|---|---|
| Part 1 | Loading the data and first checks | 3.1 |
| Part 2 | Data quality checks and cleaning | 3.2 |
| Part 3 | Re-examination of `price2`; choice of problem and algorithms | 3.3, 4.3 |
| Part 4 | Product–day panel, exploratory analysis, feature engineering | 3.4–3.6 |
| Part 5 | One-hot encoding and time-based split | 3.7 |
| Part 6 | Baselines and models, hyperparameters chosen on validation | 3.8, 4.4–4.7 |
| Part 7 | Refit on train+validation, single test evaluation | 3.9, 4.8 |
| Part 8 | Result analysis | 3.10, 4.9–4.12 |
| Part 9 | Classification view | 3.11, 4.13 |
| Part 10 | Conclusion | Conclusion |

Model labels in the notebook (partly in Vietnamese) are matched to the English names used in the report
in the report's appendix (Table 5.1).

## Method in brief

1. Clean the data: remove the truncated last day (13 Aug) and fix the category of product A18.
2. Build a complete 217-product × 134-day panel, with true zeros for days without clicks.
3. Create past-only features within each product (`lag_1`, `lag_7`, `avg_7_days`, `avg_14_days`, `day_of_week`
   and static product attributes). Every rolling feature is shifted by one day to avoid leakage.
4. Split by time: Train 15 Apr–15 Jul, Validation 16–31 Jul, Test 1–12 Aug. The test period is used once.
5. Compare 4 naive baselines with Linear Regression, Ridge, KNN, Decision Tree and Random Forest.
6. Test a classification formulation (Logistic Regression, Naive Bayes, KNN, Decision Tree, Random Forest).

## Team and contributions

| Member | Main tasks | Contribution |
|---|---|---|
| Ho Huyen Trang | Data loading and quality checks (Parts 1–2), re-examination of the `price2` approach (Part 3); Chapters 1 and 2 of the report | 33.33% |
| Ta Van Sang | Product–day table, exploratory analysis and feature engineering (Parts 4–5), leakage checks; Chapter 3 of the report | 33.33% |
| Bui Quang Minh | Models, hyperparameter selection and evaluation (Parts 6–9), classification view; Chapter 4, conclusion and slides | 33.33% |

## Repository note

<!-- Fill this in truthfully. Example: -->
The project code was first developed in <Google Colab / local notebooks> and uploaded to this repository on
<date>, so the commit history starts from that upload. Later changes (report, slides, documentation) were
committed by the member who made them.

## Data and references

Dataset: e-shop clothing 2008, UCI Machine Learning Repository (see `data/README.md` for citations).
The full reference list is in the report.
