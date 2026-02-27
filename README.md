# Landscaping SaaS Churn — Metric Association Ranking

This project analyzes a landscaping SaaS churn dataset and ranks product / usage metrics by how strongly they differ between **Active** and **Churned** accounts. It also supports segmented analysis by **ARR size** and **Persona**, with automatic sample-size checks.

## What’s in this repo

- **Notebook**: `notebooks/granum_feature_summary.ipynb`
  - Loads data using repo-friendly relative paths
  - Builds `AccountStatusBinary` from `AccountStatus` (`Active` vs `Churned`)
  - Runs an ANOVA **F-test** per numeric metric and ranks results
  - Repeats the ranking within segments:
    - `ARR Category` (ARR size) only
    - `Persona` only
    - `Persona × ARR Category` permutations
  - Enforces minimum sample size: **≥ 30 rows in each group** (Active and Churned) per segment

- **Raw data**: `data/churn_data_landscaping.csv`
- **Exports for downstream viz**: `data/exports/`
  - `f_test_overall_rank.csv`
  - `f_test_by_arr_category_rank_long.csv`
  - `f_test_by_persona_rank_long.csv`
  - `f_test_by_persona_x_arr_category_rank_long.csv`
  - Segment feasibility tables (Active/Churned counts + valid segments)

## Method (high level)

For each numeric metric, we compare **Active vs Churned** groups and compute a one-way ANOVA statistic:

- **F-statistic**: larger values indicate stronger separation between the group means relative to within-group variance
- **p-value**: significance of the observed separation under the ANOVA assumptions

Results are ranked primarily by **F** (descending), then by **p-value** (ascending).

## Tableau component (planned / in progress)

The intent is to build a Tableau dashboard on top of the exported CSVs in `data/exports/`, including:

- Overall metric ranking (F-stat + p-value)
- Segment selectors for `ARR Category` and `Persona`
- Top-N metrics per segment and per Persona×ARR permutation
- Feasibility indicators when a segment fails the **30/30** sample-size rule

## How to run

### Option A: Run the notebook

Open `notebooks/granum_feature_summary.ipynb` and select the kernel:

- **Python (data_viz_env)**

Then run cells top-to-bottom.
The notebook includes a dedicated export section that writes results to `data/exports/`.

## Dependencies

Install Python packages:

```bash
pip install -r requirements.txt
```

