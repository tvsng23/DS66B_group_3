# Contributing to the group project

This repository is maintained by Group 3 (DS66B) for a Machine Learning course project.

## Notebook ownership

Follow the report's Team Contributions table:

- **Ho Huyen Trang:** `notebooks/01_data_quality_and_price2.ipynb` — notebook Parts 1–3 (data loading/quality checks and the `price2` re-examination); report Chapters 1–2.
- **Ta Van Sang:** `notebooks/02_product_day_panel_eda_features.ipynb` — notebook Parts 4–5 (product–day panel, EDA, feature engineering and leakage checks); report Chapter 3.
- **Bui Quang Minh:** `notebooks/03_modeling_tuning_evaluation.ipynb` — notebook Parts 6–9 (models, tuning, evaluation and classification formulation); report Chapter 4, conclusion and slides.

The full source notebook is intentionally not part of the initial repository commit. Each owner should add their assigned notebook in a separate commit/branch so the history accurately records the work.

## Recommended workflow for each member

1. Accept the collaborator invitation and clone the repository using your own GitHub account.
2. Pull the latest `main` branch.
3. Create a branch named for your task, for example `notebook/01-data-quality-price2`.
4. Add only your assigned notebook and any agreed documentation changes.
5. Check `git status`, then commit with a specific message.
6. Push your branch and open a Pull Request to `main` for review. The owner or a teammate can review and merge it.

Example commands (adapt the branch, filename and commit message to your assignment):

```bash
git clone https://github.com/<OWNER>/<REPOSITORY>.git
cd <REPOSITORY>
git checkout -b notebook/01-data-quality-price2
# Save your notebook under notebooks/...
git status
git add notebooks/01_data_quality_and_price2.ipynb
git commit -m "Add data quality and price2 analysis notebook"
git push -u origin notebook/01-data-quality-price2
```

Use your own GitHub account and Git commit identity; do not share passwords or commit under another member's identity. GitHub records the commits and the Pull Request history.

## Notebook hand-offs and reproducibility

- Agree on filenames, column names, and file paths before one notebook depends on output from another.
- Record every intermediate input/output schema in `notebooks/README.md` or in the relevant notebook.
- Put generated intermediate data under `data/processed/`; by default those generated files are ignored by Git. The source CSV in `data/raw/` should remain unchanged.
- Make notebook cells run in order and describe whether downstream notebooks require an earlier notebook to be run first.
- Keep the temporal split chronological. Lag and rolling features must not include the forecast day's target.
- Review notebook outputs before committing, and do not update the report's reported metrics without rerunning the evaluation and discussing the differences with the group.

## Keep the repository clean

Do not commit virtual environments, notebook checkpoints, secrets, temporary exports, unrelated personal files, or large generated outputs. Before every commit, inspect `git status` and the staged diff.
