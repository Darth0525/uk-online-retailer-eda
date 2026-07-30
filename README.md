# UK Online Retailer — Data Quality & Exploratory Analysis

An end-to-end analysis of ~1.07M transaction line-items from a **UK-based online gift retailer**
(Dec 2009 – Dec 2011), structured the way a real data project runs: **prepare the data first,
then analyse it.**

The project is deliberately split into two notebooks so the cleaning decisions are auditable and
the analysis starts from a trusted, reproducible base:

| Notebook | What it does |
|---|---|
| **[`analysis/01_data_preparation.ipynb`](analysis/01_data_preparation.ipynb)** | Understand the grain & schema, quantify every data-quality issue, decide a treatment for each (with reasons), and save a clean, analysis-ready dataset. Documents caveats, assumptions, and recommendations to improve the data. |
| **[`analysis/02_analysis_and_insights.ipynb`](analysis/02_analysis_and_insights.ipynb)** | Explore the prepared data and tell the story: revenue & seasonality, geography, customer concentration, products & categories, pricing, returns — then **RFM segmentation, cohort retention, market-basket affinity, and a repeat-purchase model.** Ends with findings (with confidence levels) and recommendations. |

## Headline findings
- **Revenue is highly concentrated** — the UK is ~86% of revenue, the **top 20% of customers
  drive ~77%**, and the same product sells cheaper in bulk (volume/wholesale pricing).
- **Demand peaks in November**; December is truncated (Christmas shutdown), so it can't be read
  as the peak.
- **Merchandise returns are modest but rising** — ~2.5% (2010) → ~4.8% (2011).
- **Purchasing is irregular but repeat-heavy** — only ~21% of a cohort reorders the next month,
  yet ~72% reorder within two years; a first-order model predicts repeat only weakly (AUC ≈ 0.68).
- **Kitchen & Dining (~21%) and Home Décor (~16%)** lead the (text-derived) product mix, with
  clear product affinities for bundling.

## What this project demonstrates
- **Data-quality-first workflow** — issues are quantified and treated *before* any business claim,
  with a documented, reversible decision for each.
- **Honest analytical judgment** — explicit confidence levels and caveats (e.g. exact duplicates
  are ~89% concentrated in Dec 2010 and flip the December seasonality read; ~23% of rows are
  unattributed and excluded from customer-level analysis).
- **Range** — from EDA to RFM, cohort retention, market-basket affinity, and a simple predictive
  baseline (scikit-learn) framed honestly.
- **Reproducibility** — every number is computed in-notebook from the raw file; the data-prep
  stage hands a versioned, typed dataset to the analysis stage.

## Reproduce
```bash
python -m venv .venv && source .venv/bin/activate
pip install -r requirements.txt

# Notebook 1 downloads the dataset from Kaggle on first run (needs ~/.kaggle/kaggle.json),
# then writes data/online_retail_prepared.parquet
jupyter lab analysis/01_data_preparation.ipynb   # Restart & Run All
jupyter lab analysis/02_analysis_and_insights.ipynb           # Restart & Run All
```
Run Notebook 1 first (it produces the prepared dataset Notebook 2 loads).

## Structure
```
uk-online-retailer-eda/
├── analysis/
│   ├── 01_data_preparation.ipynb   # prepare → data/online_retail_prepared.parquet
│   └── 02_analysis_and_insights.ipynb           # EDA + RFM + cohorts + basket + model
├── requirements.txt
└── data/                                       # gitignored (raw + prepared; regenerated)
```

## Data
[Online Retail II](https://www.kaggle.com/datasets/mashlyn/online-retail-ii-uci) (UCI Machine
Learning Repository, CC0). One row = one invoice line item; the notebooks derive revenue, classify
line types, and separate cancellations. Tools: Python (pandas, numpy, matplotlib, seaborn,
scikit-learn), Jupyter.
