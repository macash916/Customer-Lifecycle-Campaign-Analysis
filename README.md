# Customer Lifecycle & Campaign Analysis

**Author:** Amrit Sharma
**Dataset:** Online Retail II (UCI Machine Learning Repository) — UK gift & homeware retailer, 2009–2011  
**Stack:** Python · pandas · scikit-learn · SQLite · Matplotlib · Klaviyo · Google Ads

---

## Project Overview

End-to-end customer analytics pipeline built on 779,425 clean transactions across 5,878 customers. The project moves from raw data to a fully costed campaign strategy across five analytical notebooks, culminating in a Customer Lifetime Value model that quantifies the forward revenue value of every customer.

**Key outputs:**
- £6.7M projected 12-month portfolio CLV across 5,878 customers
- £445K forward CLV at measurable churn risk (revised from £5.5M historical-spend figure)
- 7-segment RFM model driving all campaign decisions
- Churn model (AUC 0.777) with identified model blind spot documented
- £12,500/month Google Ads campaign plan with audience lists and bid strategies
- Top 50 priority retention list (£2.5M combined CLV — 37.9% of portfolio)

---

## Notebooks

| Notebook | Description | Key Output |
|---|---|---|
| `01_eda.ipynb` | Exploratory data analysis — cleaning, distributions, seasonality, Pareto analysis | 8 charts · peak trading: Thursday 11am–1pm |
| `02_rfm_segmentation.ipynb` | RFM scoring and 7-segment customer classification | `rfm_segments.csv` · segment revenue breakdown |
| `02b_sql_analysis.ipynb` | 8 SQL queries against SQLite (785,303 rows) validating Python findings independently | SQL confirmation of RFM results |
| `03_churn_model.ipynb` | Logistic Regression vs Random Forest churn prediction (90-day window) | AUC 0.777 · `rfm_with_churn.csv` · documented At Risk blind spot |
| `04_campaign_attribution.ipynb` | First-touch, last-touch, and linear attribution across 3 channels | Attribution comparison chart · Paid Search ranked #1 across all models |
| `05_clv_model.ipynb` | Survival-weighted DCF Customer Lifetime Value model at 12M and 24M horizons | `rfm_with_clv.csv` · `top50_priority_retention.csv` · £6.7M portfolio CLV |

---

## Key Results

### Customer Lifetime Value (Notebook 05)

| Metric | Value |
|---|---|
| Total portfolio CLV 12M | £6,706,263 |
| Total portfolio CLV 24M | £10,714,196 |
| Champions CLV 12M | £4,786,184 (71.4% of portfolio) |
| At-Risk forward CLV | £445,269 (3,275 customers, churn prob >50%) |
| Top 50 customers CLV | £2,544,523 (37.9% of portfolio) |
| At Risk mean CLV | £3,373 (near-Champion level — 89 customers) |

### Churn Model (Notebook 03)

| Metric | Value |
|---|---|
| Model selected | Logistic Regression |
| ROC-AUC | 0.777 |
| Churn definition | No purchase in 90 days |
| Top feature | Frequency (importance 0.41) |
| Known limitation | At Risk segment underscored — Recency excluded to prevent data leakage |

### RFM Segmentation (Notebook 02)

| Segment | Customers | Historical Revenue | CLV 12M |
|---|---|---|---|
| Champions | 1,482 | £12.0M | £4,786,184 |
| Loyal Customers | 1,221 | £2.5M | £697,630 |
| Need Attention | 551 | £1.1M | £507,002 |
| At Risk | 89 | £269K | £300,206 |
| Lost | 1,523 | £654K | £250,593 |
| New Customers | 184 | — | £97,726 |
| Potential Loyal | 828 | — | £66,921 |

---

## Campaign Strategy

### Klaviyo Flows

| Segment | Flow | Trigger |
|---|---|---|
| Champions (early warning) | VIP early access | 38.5% churn probability flag |
| Loyal Customers | Loyalty milestone reward | Purchase interval lapse |
| Need Attention | Time-limited re-engagement | Urgency + scarcity framing |
| At Risk | Manual personal outreach | CRM override — model blind spot |
| Lost | Final win-back + suppress | One 20% offer, then permanent suppression |

### Google Ads (£12,500/month)

| Campaign | Segment | Budget | Strategy |
|---|---|---|---|
| Champions Retention | 512 customers | £3,500 | Customer Match +30% bid · Target ROAS 600% |
| Loyal RLSA Boost | 1,221 customers | £4,000 | RLSA +20% · Target CPA £18 |
| Win-Back Display | 551 customers | £3,500 | 3-phase creative · Target CPA £18 |
| Lost Suppression | 1,523 customers | £1,500 | Negative audiences · final email only |

**At Risk (89 customers):** Excluded from all paid campaigns. Bespoke Klaviyo outreach only.  
**Bid scheduling:** Thursday 09:00–13:00 +25–30% across all Search campaigns (EDA peak trading window).

---

## Output Files

### Charts (`outputs/`)

| File | Notebook | Description |
|---|---|---|
| `monthly_revenue_trend.png` | 01 | Revenue by month across 2009–2011 |
| `top_countries_revenue.png` | 01 | Top international markets by revenue |
| `top_products_revenue.png` | 01 | Top 20 products by total revenue |
| `purchase_timing_behaviour.png` | 01 | Heatmap: hour × day of week purchase volume |
| `customer_revenue_distribution.png` | 01 | Revenue distribution — Pareto skew visualised |
| `segment_size_revenue.png` | 02 | Customer count and revenue by RFM segment |
| `rfm_distribution_boxplot.png` | 02 | Boxplot of R, F, M distributions per segment |
| `customer_map_scatter.png` | 02 | RFM scatter — Recency vs Frequency coloured by segment |
| `rfm_heatmap.png` | 02 | Heatmap of mean RFM scores per segment |
| `pareto_curve.png` | 02 | Lorenz curve — historical revenue concentration |
| `roc_curve_comparison.png` | 03 | ROC curves: Logistic Regression vs Random Forest |
| `feature_importance.png` | 03 | Feature importance from Random Forest (benchmark) |
| `attribution_comparison.png` | 04 | First-touch vs last-touch vs linear — revenue share by channel |
| `clv_by_segment.png` | 05 | Total and mean CLV 12M by segment |
| `clv_concentration.png` | 05 | CLV distribution histogram + Lorenz concentration curve |
| `clv_churn_matrix.png` | 05 | CLV vs Churn Probability — strategic intervention 2×2 |
| `clv_12m_vs_24m.png` | 05 | Mean CLV at 12M and 24M horizons per segment |
| `clv_executive_summary.png` | 05 | Executive dashboard: 4-panel CLV summary |

### Data (`data/`)

Data files are excluded from this repository (`.gitignore`). To reproduce:

1. Download `online_retail_II.xlsx` from the [UCI Machine Learning Repository](https://archive.ics.uci.edu/dataset/502/online+retail+ii)
2. Place in `data/` and run notebooks in order (01 → 02 → 03 → 04 → 05)
3. Each notebook outputs its own CSV to `data/` for the next notebook to consume

| File | Generated by | Used by |
|---|---|---|
| `online_retail_clean.csv` | 01 | 02, 02b |
| `rfm_segments.csv` | 02 | 02b, 03 |
| `rfm_with_churn.csv` | 03 | 04, 05 |
| `rfm_with_clv.csv` | 05 | CRM / Google Ads upload |
| `top50_priority_retention.csv` | 05 | Manual CRM outreach |

---

## Repository Structure

```
Customer-Lifecycle-Campaign-Analysis/
├── notebooks/
│   ├── 01_eda.ipynb
│   ├── 02_rfm_segmentation.ipynb
│   ├── 02b_sql_analysis.ipynb
│   ├── 03_churn_model.ipynb
│   ├── 04_campaign_attribution.ipynb
│   └── 05_clv_model.ipynb
├── outputs/
│   └── [18 chart PNGs — see Output Files section]
├── strategy_recommendations.md
├── requirements.txt
└── README.md
```

---

## Setup

```bash
git clone https://github.com/macash916/Customer-Lifecycle-Campaign-Analysis.git
cd Customer-Lifecycle-Campaign-Analysis
pip install -r requirements.txt

# Download data from UCI and place in data/
# Then run notebooks in order
```

**Requirements:** pandas · numpy · matplotlib · scikit-learn · seaborn · openpyxl · sqlite3 (stdlib)

---

## Methodological Notes

**Data cleaning:** 22.77% of raw rows removed — anonymous transactions (no Customer ID), cancellations (invoice prefix C), duplicates, and zero/negative price rows.

**RFM scoring:** Quintile-based (1–5) using `pd.qcut`. Scores are relative to the full customer base — a score of 5 means top 20% of all customers, not an absolute threshold.

**Churn model:** Logistic Regression (AUC 0.777) selected over Random Forest (AUC 0.773) for interpretability. Recency excluded from features — churn defined as Recency > 90 days, making Recency a direct target leakage risk. The At Risk segment is consequently underscored and requires manual CRM override (see `strategy_recommendations.md`).

**CLV model:** Survival-weighted DCF. Monthly survival probability derived from 90-day churn output: P(survive month) = (1 − P90d)^(1/3). Discount rate 10% annual. In production, BG/NBD + Gamma-Gamma (lifetimes library) on raw transaction data would be preferred.

**Attribution:** Channel touchpoints simulated using RFM behavioural proxies — methodology demonstration. Production implementation uses GA4 API event-level data.
