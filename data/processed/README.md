# Processed data

Outputs of [`notebooks/01_clean_trade_agreements.ipynb`](../../notebooks/01_clean_trade_agreements.ipynb), which builds the African–Northern trade-agreement panel described in `docs/developing-country-trade-productivity.md` §5–§6. Re-run that notebook to regenerate these files from the raw source in `data/baier-bergstrand/`.

## `baier_bergstrand_africa_northern_country_year.csv`

The primary analysis-ready panel: one row per (African country, year), 1950–2017. This is what gets merged with the GGDC sectoral productivity panel in the next notebook.

| Column | Meaning |
|---|---|
| `iso3` | African country's ISO3 code (24 countries, per methodology doc §5.1) |
| `year` | 1950–2017 |
| `country_exists` | `False` if the country was not yet a recognized state per Baier–Bergstrand's own coding that year (see their `NoCty` designation) |
| `depth_score` | Deepest (max) Baier–Bergstrand integration level (0–6) reached with *any* Northern partner that year; `NaN` where `country_exists` is `False` |
| `agreement_type` | Categorical label for `depth_score` (`0_none` … `6_economic_union`) |
| `reciprocal` | **The primary treatment variable** (methodology doc §6.2): 1 if `depth_score >= 2`, else 0 |
| `fta_or_deeper` | 1 if `depth_score >= 3`; a heterogeneity/robustness cut (§6.4.D), not the primary treatment |
| `nonreciprocal_only` | 1 if `depth_score == 1` exactly (GSP/AGOA-type non-reciprocal preferences); a heterogeneity cut, not the primary treatment |
| `n_northern_reporting` | Number of the 36 Northern partners with a non-`NoCty` value that year (out of 36; less than 36 mainly reflects Northern partners that didn't yet exist as recognized states, e.g. Baltic states pre-1991) |
| `n_northern_with_agreement` | Of those reporting, how many show `level >= 1` |

**Diagnostics run on this panel and confirmed in the notebook:** `reciprocal` never reverts from 1 to 0 for any of the 24 countries over 1950–2017 (empirically absorbing); `nonreciprocal_only` does show 17 apparent reversions, but every one is a graduation to reciprocal status, not a genuine loss of preference, within this specific country set and window.

## `baier_bergstrand_africa_northern_bilateral.csv`

The intermediate bilateral panel this is built from: one row per (African country, Northern partner, year) — 24 × 36 × up to 68 years. Kept for future extensions that need partner-level detail (e.g., trade- or GDP-weighted exposure, per methodology doc §6.2 step 5, once bilateral trade-flow data is added).

| Column | Meaning |
|---|---|
| `african_iso3`, `northern_iso3` | ISO3 codes |
| `year` | 1950–2017 |
| `level` | Baier–Bergstrand integration level (0–6), collapsed from the two directional rows by taking the max — see the notebook for why this is the economically correct collapse rule for this project (non-reciprocal preferences are directional; reciprocal levels already agree in both directions) |
| `not_yet_country` | Whether either direction was `NoCty` |
