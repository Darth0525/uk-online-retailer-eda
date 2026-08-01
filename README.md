# UK Online Retailer — Data Quality & Exploratory Analysis

An end-to-end analysis of ~1.07M transaction line-items from a **UK-based online gift retailer**
(Dec 2009 – Dec 2011), structured the way a real data project runs: **prepare the data first,
then analyse it.**

The project is split into two notebooks so the cleaning decisions are auditable and the analysis
starts from a trusted, reproducible base:

| Notebook | What it does |
|---|---|
| **[`analysis/01_data_preparation.ipynb`](analysis/01_data_preparation.ipynb)** | Understand the grain & schema; quantify every data-quality issue; **derive** which stock codes are services vs. products; decide and apply a treatment for each issue (dedupe, classify line types, impute, type-cast); save a clean, analysis-ready dataset. Documents caveats, assumptions, and concrete recommendations to improve the data. |
| **[`analysis/02_analysis_and_insights.ipynb`](analysis/02_analysis_and_insights.ipynb)** | Tell the story the data supports: **revenue & seasonality → geography → customers → products & categories → pricing → returns**, then a **deeper analysis** layer. |

**Notebook 2 covers:**
- **Revenue over time & seasonality** (per-active-day view; November peak; December caveat)
- **Geographic concentration** (UK vs. rest)
- **Customers** — revenue concentration (Pareto) and **new vs. returning / the active base**
- **Products & categories** — top sellers, category mix, **category trends over time**, and **emerging vs. declining** categories
- **Pricing behaviour** (volume / wholesale discounting)
- **Returns** (rate over time + high-cancellation tail)
- **Deeper analysis** — **RFM segmentation**, **cohort retention**, **market-basket affinity**, and a **repeat-purchase model** (scikit-learn)
- **Findings** (with confidence levels) and **recommendations**

## Headline findings
- **Revenue is highly concentrated** — the UK is about 86% of revenue, the top 20% of customers
  drive about 77%, and RFM "Champions" (about 25% of customers) alone are about 69%. The same
  product sells cheaper in bulk (volume/wholesale pricing).
- **Demand peaks in November**; December is truncated (Christmas shutdown), so it can't be read as
  the peak.
- **The business runs on returning customers** — about 86% of revenue — and the active base is
  roughly flat year-over-year, so growth depends on **retention/reactivation**, not just acquisition.
- **Purchasing is irregular but repeat-heavy** — only about 17–20% of a cohort reorders in a given
  month, yet about 72% reorder within two years (a first-order model predicts repeat only weakly).
- **Category growth is broad in gifting/bags** (Kids +18%, Bags +15%, Christmas +14%) while
  **Garden (−35%), Stationery (−17%) and Lighting (−11%) decline**; Kitchen & Dining + Home Décor
  are about 37% of sales.
- **Strong product-range affinities** (lift 7–10×) point to clear bundling / cross-sell opportunities.

## What this project demonstrates
- **Data-quality-first workflow** — issues are quantified, and each treatment is a *documented,
  reversible decision* (including how the service-code list was derived, not hardcoded).
- **Honest analytical judgment** — explicit data-sufficiency and validity notes (e.g. cohort
  **left-censoring** and irregular **wholesale reorder cadence**; ~23% of rows are unattributed and
  excluded from customer-level work; exact-duplicate ambiguity concentrated in Dec 2010).
- **Breadth** — from EDA to **RFM segmentation, cohort retention, market-basket affinity, trend /
  emerging-vs-declining analysis, and a predictive baseline** — each framed with its limits.
- **Reproducibility** — a two-stage pipeline (prep → analysis); every number is computed
  in-notebook from the raw file.

## Reproduce
```bash
python -m venv .venv && source .venv/bin/activate
pip install -r requirements.txt

# Notebook 1 downloads the dataset from Kaggle on first run (needs ~/.kaggle/kaggle.json),
# then writes data/online_retail_prepared.parquet
jupyter lab analysis/01_data_preparation.ipynb      # Restart & Run All
jupyter lab analysis/02_analysis_and_insights.ipynb # Restart & Run All
```
Run Notebook 1 first — it produces the prepared dataset that Notebook 2 loads.

## Structure
```
uk-online-retailer-eda/
├── analysis/
│   ├── 01_data_preparation.ipynb       # prepare -> data/online_retail_prepared.parquet
│   └── 02_analysis_and_insights.ipynb  # EDA + RFM + cohorts + basket + trends + model
├── requirements.txt
└── data/                               # gitignored (raw + prepared; regenerated)
```

## Data
[Online Retail II](https://www.kaggle.com/datasets/mashlyn/online-retail-ii-uci) (UCI Machine
Learning Repository, CC0). One row = one invoice line item; the notebooks derive revenue, classify
line types, and separate cancellations. Tools: Python (pandas, numpy, matplotlib, seaborn,
scikit-learn), Jupyter.
