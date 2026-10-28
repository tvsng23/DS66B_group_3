# Notebook plan and hand-offs

The notebooks below are intentionally **not committed yet**. Each assigned member should create and commit their notebook under the agreed filename. Their scope follows the report's Team Contributions table.

| Order | Planned filename | Owner | Scope |
|---|---|---|---|
| 1 | `01_data_quality_and_price2.ipynb` | Ho Huyen Trang | Data loading, quality checks and `price2` re-examination (Parts 1–3). |
| 2 | `02_product_day_panel_eda_features.ipynb` | Ta Van Sang | Product–day panel, EDA, feature engineering and leakage checks (Parts 4–5). |
| 3 | `03_modeling_tuning_evaluation.ipynb` | Bui Quang Minh | Baselines/models, tuning, final evaluation and classification formulation (Parts 6–9). |

## Shared contracts to agree before implementation

- **Input source:** `data/raw/e-shop-clothing-2008.csv` (semicolon-delimited).
- **Intermediate files:** if one notebook passes processed data to another, save it under `data/processed/` and document its exact name, columns, and how it was generated.
- **Execution order:** state explicitly at the top of each notebook whether the previous notebook needs to be run first.
- **Forecast target:** number of product views on the forecast day. Lag/rolling features must use prior days only.
- **Evaluation:** use the chronological train/validation/test periods documented in the report; do not tune on the test set.

Do not duplicate a full notebook across all three files. Each file should contain the work that belongs to that member and any minimal preparation needed to make its own scope reproducible.
