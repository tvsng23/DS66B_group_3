# Forecasting Daily Product Views of an Online Clothing Store from Clickstream Data

Course project for **Machine Learning**, Group 3, class DS66B, School of Technology, National Economics University (NEU).

> **Repository setup status:** This is the initial project scaffold. The full notebook has intentionally **not** been uploaded. The team will add three separately maintained notebooks, following the responsibilities listed below, so each member's work can be reviewed through Git commits and pull requests.

## Project overview

The project uses the public **Clickstream Data for Online Shopping / e-shop clothing 2008** dataset to forecast the next day's number of views for each product. Views are a measure of attention, not purchases or sales. The intended use is short-horizon online merchandising rather than inventory planning.

The report also re-examines the common `price2` classification task to demonstrate how target leakage and product memorisation can create misleadingly high scores.

## Planned notebook split and team responsibilities

The division below follows the **Team Contributions** section of the report. Notebook filenames are the proposed repository naming convention; members should use these names unless the group agrees on a change.

| Notebook | Owner | Scope from the report |
|---|---|---|
| `notebooks/01_data_quality_and_price2.ipynb` | **Ho Huyen Trang** | Notebook Parts 1–3: data loading and quality checks; re-examination of the `price2` classification task. Report Chapters 1–2. |
| `notebooks/02_product_day_panel_eda_features.ipynb` | **Ta Van Sang** | Notebook Parts 4–5: product–day panel, exploratory data analysis, feature engineering and leakage checks. Report Chapter 3. |
| `notebooks/03_modeling_tuning_evaluation.ipynb` | **Bui Quang Minh** | Notebook Parts 6–9: models, hyperparameter selection, final evaluation and classification formulation. Report Chapter 4, conclusion and slides. |

The notebooks are not included yet. Before finalising them, agree on how data will pass between notebooks. In particular, document the path and schema of any processed file created by Notebook 1 or 2 and consumed by a later notebook. Generated files belong under `data/processed/` and are ignored by Git by default; regenerate them by running the required earlier notebook(s).

## Repository structure

```text
.
├── README.md
├── requirements.txt
├── .gitignore
├── .gitattributes
├── CITATION.cff
├── CONTRIBUTING.md
├── data/
│   ├── README.md
│   ├── raw/
│   │   └── e-shop-clothing-2008.csv
│   └── processed/
│       └── README.md
├── docs/
│   ├── Report_EN_v5_3_optimized.pdf
│   └── SETUP_VI.md
└── notebooks/
    └── README.md
```

## Getting started

### 1. Environment

Python 3.10–3.12 is recommended. From the repository root:

**Windows PowerShell**

```powershell
python -m venv .venv
.\.venv\Scripts\Activate.ps1
python -m pip install --upgrade pip
pip install -r requirements.txt
```

If PowerShell blocks activation, use Command Prompt and run `.venv\Scripts\activate.bat`.

**macOS / Linux**

```bash
python3 -m venv .venv
source .venv/bin/activate
python -m pip install --upgrade pip
pip install -r requirements.txt
```

### 2. Dataset

The source CSV is stored at `data/raw/e-shop-clothing-2008.csv` and is semicolon-delimited (`;`). Keep the raw file unchanged; apply cleaning and corrections in notebooks or to files under `data/processed/`.

### 3. Notebooks

The notebook files will be added by their assigned owners. See `notebooks/README.md` and `CONTRIBUTING.md` for their expected scope and the recommended workflow. Until those files are committed, the analysis cannot be reproduced from this repository alone; the included report provides the current written account of the study.

## Current reported findings

The supplied report documents the following test-set results. These are report-reported results and should be revalidated against the final split notebooks before submission.

| Method | Test MAE (views per product per day) | Test R² |
|---|---:|---:|
| 14-day average baseline | 2.126 | 0.686 |
| Linear Regression, all features | 2.078 | 0.701 |
| Ridge Regression | 2.078 | 0.701 |
| K-Nearest Neighbours | 2.043 | 0.702 |
| Decision Tree | 2.053 | 0.701 |
| **Random Forest** | **2.023** | **0.711** |

According to the report, Random Forest reduces MAE by 4.8% against the strongest baseline, but beats it on only 7 of the 12 test days. The report therefore characterises the improvement as promising but not conclusive.

## Data source and attribution

The dataset is available from the [UCI Machine Learning Repository: Clickstream Data for Online Shopping](https://archive.ics.uci.edu/dataset/553/clickstream%2Bdata%2Bfor%2Bonline%2Bshopping), DOI [10.24432/C5QK7X](https://doi.org/10.24432/C5QK7X). UCI lists the dataset under CC BY 4.0. Retain attribution when redistributing it and check UCI's current terms.

The English report supplied with this project is stored in `docs/Report_EN_v5_3_optimized.pdf`.

## Reproducibility and quality rules

- This dataset records product-page views, not purchases.
- Forecasting features must use only information available before the forecast day. Rolling means must be shifted to prevent target leakage.
- Keep the forecasting split chronological; do not randomly split product–day rows.
- Run and review notebooks in dependency order, and record input/output paths between them.
- The report refers to a notebook named `DS66B_group_3.ipynb`, while the uploaded source notebook was named `EShop_StepByStep_Student_v5.ipynb`. The team should reconcile notebook naming/version references before final submission.
- The report cover states 30 October 2026. Confirm that date before publishing the final report.

## License

No licence has been chosen for the group's original report and code. The dataset's licence does not automatically apply to the group's original materials. Agree as a group before adding a repository-level `LICENSE` file.
