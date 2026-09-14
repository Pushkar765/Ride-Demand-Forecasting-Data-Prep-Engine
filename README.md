# 🚕 Ride Demand Forecasting — Data Prep Engine

A single-notebook pipeline that takes messy, multi-source ride-hailing data and turns it into a clean, feature-rich dataset ready for demand forecasting models.

## 📊 What this notebook does

Rider, trip, and zone data arrive from three different sources in three different formats — CSV, JSON, and SQL — each with its own quirks: missing values, inconsistent dates, dodgy entries, and skewed distributions. This notebook ingests all three, cleans them up, engineers useful features, and merges everything into one export-ready dataset, with a before/after summary and a couple of exploratory charts along the way.

## 🧪 Key features / analyses

- **Multi-source ingestion** — loads and inspects rider (CSV), trip (JSON), and zone (SQL) datasets side by side, checking `.info()`, missing values, duplicates, and invalid entries
- **Data cleaning** — mean imputation for numeric gaps, most-frequent imputation for categoricals, KNN imputation for correlated fields (duration, distance, fare), date normalisation, and removal of unrealistic rows (negative fares, billed zero-distance rides)
- **Outlier handling** — Z-score detection for fare/distance anomalies, IQR for duration, and Winsorization for extreme surge fares
- **Data transformation** — datetime decomposition (hour/day/month), Label/One-Hot/Ordinal encoding, frequency binning, and log/sqrt transforms for skewed columns
- **Feature scaling** — Standard and MinMax scaling of numeric features
- **Feature engineering** — average ride distance/fare, peak-hour flag, days since signup, cancellation rate, and a refined surge flag
- **Merge & export** — joins all three sources into one final dataset with a before/after summary table, written out as `final_prepared_rides_dataset.csv`
- **Visualisations** 📈 — surge vs no-surge fare comparison and other exploratory plots

## 🤖 Requirements

- Python 3.11+
- `pandas`, `numpy`, `matplotlib`, `seaborn`
- `scikit-learn` (imputers, encoders, scalers, `ColumnTransformer`)
- `scipy`
- `ydata-profiling`
- `sqlite3` (standard library), `requests`, `json`, `os`

## 🗺️ Setup & run

```bash
# Clone the repo
git clone https://github.com/[YOUR_USERNAME]/[REPO_NAME].git
cd [REPO_NAME]

# Create and activate a virtual environment
python -m venv .venv
source .venv/bin/activate      # Windows: .venv\Scripts\activate

# Install dependencies
pip install pandas numpy matplotlib seaborn scikit-learn scipy ydata-profiling requests

# Launch Jupyter
jupyter notebook Ride_Demand_Forecasting_Data_Prep_Engine.ipynb
```

## 🧠 How to use

1. Place the source data — [DATASET].csv, [DATASET].json, and a SQLite DB (or update the SQL connection string) — in the expected input path.
2. Run the notebook top to bottom, section by section (1–8), as each stage builds on the last.
3. Inspect the before/after summary table in Section 7 to see what changed.
4. Find your cleaned, feature-engineered output at `final_prepared_rides_dataset.csv`, ready to feed into a forecasting model.

## 📈 Results / insights

- Missing values were reduced from 20 to 18 across the merged dataset after imputation
- 19 outliers were identified and removed via Z-score/IQR methods, with none flagged before cleaning
- 6 new engineered features were added (avg ride distance, avg ride fare, peak-hour flag, days since signup, cancellation rate, surge flag)
- Row count held steady at 2,000 through the pipeline — no data loss from the cleaning steps
- Surge vs non-surge trips show a visibly different average fare, as seen in the final visualisation

## 🔧 How to extend / next steps

- Feed `final_prepared_rides_dataset.csv` into a forecasting model (e.g. XGBoost, Prophet, or an LSTM) to predict demand by zone/hour
- Add time-series cross-validation rather than a single train/test split
- Automate the SQL/JSON/CSV ingestion with a config file instead of hardcoded paths
- Extend the `ydata-profiling` report into a full EDA dashboard
- Add unit tests for the cleaning and feature-engineering functions

## 📄 License

MIT — see [LICENSE](LICENSE) for details.

## 🙏 References / acknowledgements

- [scikit-learn documentation](https://scikit-learn.org/stable/)
- [ydata-profiling](https://github.com/ydataai/ydata-profiling)
- [DATASET SOURCE] — if applicable, credit the original data source here
