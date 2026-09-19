# Data Science Job Market: Salary vs. Satisfaction

A cleaning-to-insight analysis of ~700 real data science job postings, asking one business question: **do the highest-paying industries for data scientists also have the happiest employees — or is there an opening to compete on culture instead of pay?**

## The finding

Filtering to Data Scientist–titled postings with at least 5 listings in a category (to avoid a 1-2 posting sample skewing an average), the five highest-paying industry categories are:

| Industry Category | Avg. Salary | Avg. Rating | Postings |
|---|---:|---:|---:|
| Retail & Consumer Goods | **$158,200** | **3.31** | 10 |
| Manufacturing | $135,636 | 3.62 | 44 |
| Education & Public Sector | $131,308 | 3.82 | 13 |
| Business & Consulting Services | $128,404 | 3.97 | 83 |
| Healthcare & Biotech | $125,233 | 3.67 | 43 |

**Retail & Consumer Goods pays the most — by a wide margin — but has the lowest employee satisfaction rating of the top 5.** Every other top-paying category clears a 3.6+ rating; Retail & Consumer Goods sits at 3.31. That's a real, if small-sample (10 postings), signal for a company in that space: pay alone isn't buying the same employee satisfaction it's buying elsewhere, which is either a warning sign or an opening — a company willing to invest in culture rather than just compensation could stand out in a category where the current leader isn't winning on happiness.

## Data

`data/Uncleaned_DS_jobs.csv` — 672 real Glassdoor job postings scraped for a public Kaggle dataset, deliberately messy (placeholder `-1` values, glued-together fields, inconsistent formatting) before cleaning.

## Method

Full step-by-step process, with reasoning, is in [`salary_satisfaction_analysis.ipynb`](salary_satisfaction_analysis.ipynb):

1. **Placeholder cleanup** — `-1` and `"Unknown / Non-Applicable"` replaced with real nulls (handled separately for the numeric `Rating` column, whose own `-1.0` placeholder wouldn't match a string check).
2. **Duplicate detection** — the raw file's leading `index` column made every row look unique to a naive check; excluding it surfaced 13 real duplicate postings, which were dropped (672 → 659 rows) to avoid double-counting in salary/rating averages.
3. **Rating imputation** — missing ratings filled with their industry's own median rather than a global mean, since different industries have different baseline rating distributions, and a median is robust to a handful of unusually high or low ratings pulling a mean fill toward them.
4. **Salary parsing** — the raw `"$137K-$171K (Glassdoor est.)"` format split into numeric min/max/average columns.
5. **Industry consolidation** — 57 raw, free-text industry values mapped into 12 broad, human-readable categories via an explicit function (not a black-box model), so the grouping logic is fully reviewable.
6. **Minimum-sample filtering** — any industry category with fewer than 5 postings excluded from the ranking, so a 1-2 posting category can't produce a misleadingly extreme average.

## Reproduce it

Originally developed in Google Colab; adapted to read `data/Uncleaned_DS_jobs.csv` from this repo so it runs standalone:

```bash
python3 -m venv .venv && source .venv/bin/activate
pip install pandas numpy matplotlib seaborn plotly jupyter nbconvert adjustText
jupyter nbconvert --to notebook --execute --inplace salary_satisfaction_analysis.ipynb
```

## Tools

Python, pandas, seaborn/matplotlib.

## License

MIT — see [LICENSE](LICENSE).
