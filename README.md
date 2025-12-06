## ISI Inference – Aircraft Bird Strikes

Data- and inference-focused exploration of the FAA Wildlife Strike Database with an emphasis on gull encounters. The project combines fast ingestion via Polars with pandas/SciPy tooling to quantify where, when, and how gull strikes lead to aircraft damage.

### Project Highlights
- **Dataset**: `data/wildlife-strikes/database.csv` produced from FAA Wildlife Strike downloads (run `notebooks/download_wildlife.ipynb` to refresh).
- **Scope**: Global EDA + gull-specific drill downs (flight phases, states, aircraft components, seasonality) followed by formal hypothesis tests.
- **Stack**: Python 3.12, Polars for lazy scans, pandas/Numpy for manipulation, seaborn/matplotlib for visuals, SciPy & statsmodels for inference, uv for environment management.

### Repository Structure
- `main.py` – entry point (placeholder for automated runs / CLI wiring).
- `notebooks/` – analysis notebooks:
  - `polars_exploration.ipynb`: broad FAA strike EDA (damage mix, categorical coverage, yearly trends).
  - `gull_analysis.ipynb`: descriptive analytics on gull strikes (phases, states, struck components, seasonality, species×state heatmaps).
  - `gull_hypothesis_tests.ipynb`: chi-square tables, two-proportion z-tests, and Kruskal–Wallis checks for gull-specific hypotheses.
  - `download_wildlife.ipynb`: helper to pull the latest FAA CSV and store it under `data/wildlife-strikes/`.
- `pyproject.toml` / `uv.lock` – reproducible dependency definitions.
- `kaggle.json` – Kaggle credentials stub (required only if you mirror the dataset via Kaggle).

### Key Analytical Findings (current snapshot)
- **Population**: 10,584 gull strikes with an overall 13.73% reported damage rate after cleaning null categories.
- **Phase effects**: Chi-square test (χ² ≈ 1090, p < 1e-16) rejects the hypothesis that damage is phase-independent; EN ROUTE strikes dominate the damage share.
- **Geography**: Among the eight busiest states, χ² ≈ 17.1 (p ≈ 0.016) indicates non-uniform damage risk, motivating state-specific mitigation.
- **Species & Components**: χ² > 140 for top gull species and χ² > 720 for struck components show clear heterogeneity; lighting assemblies are especially fragile.
- **Targeted contrasts**: Two-proportion tests reveal EN ROUTE strikes have ~64.6% damage rate vs ~17.0% on APPROACH (z ≈ 14.1, p < 1e-40) and Lights vs Wing/Rotor components differ sharply (86.0% vs 28.5%, z ≈ 11.6, p < 1e-30). Peak-month damage rates are statistically similar to off-peak at α = 0.05 (p ≈ 0.066).
- **Distributional checks**: Kruskal–Wallis tests confirm median damage propensity differs across phases (H ≈ 438), top states (H ≈ 26), and months (H ≈ 46), reinforcing the categorical findings without parametric assumptions.

### Reproducing the Environment
1. Install [uv](https://github.com/astral-sh/uv) (or select another PEP 621-compatible tool).
2. From repo root:
	```bash
	uv sync
	```
3. Launch notebooks with:
	```bash
	uv run jupyter lab
	```

### Refreshing the Dataset
1. Ensure `data/wildlife-strikes/` exists.
2. Open `notebooks/download_wildlife.ipynb` and run all cells. The notebook downloads/extracts the official FAA CSV into both `data/` (for scripts) and `notebooks/data/` (for interactive work).

### Extending the Analysis
- Add post-hoc pairwise tests (e.g., Dunn or pairwise proportion tests) where Kruskal/chi-square flagged significance.
- Incorporate time-of-day, airport class, or aircraft weight-class filters.
- Productionize alerts by converting the gull hypothesis notebook into a scheduled pipeline (e.g., with Prefect/Airflow) that flags high-risk states/components monthly.

### License & Contributions
No license is specified yet; reach out via GitHub issues for usage questions or to share improvements. PRs that broaden statistical coverage or harden the data pipeline are especially welcome.
