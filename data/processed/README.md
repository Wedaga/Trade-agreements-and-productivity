# Processed data

Outputs of two notebooks, run in order:

1. [`notebooks/01_clean_trade_agreements.ipynb`](../../notebooks/01_clean_trade_agreements.ipynb) — builds the African–Northern trade-agreement panel described in `docs/developing-country-trade-productivity.md` §5–§6, from the raw source in `data/baier-bergstrand/`.
2. [`notebooks/02_clean_cepii_gravity.ipynb`](../../notebooks/02_clean_cepii_gravity.ipynb) — builds the trade- and GDP-weighted exposure variables (methodology doc §6.2 step 5), from the raw source in `data/cepii-gravity/`, and merges them onto notebook 1's country–year panel.

Re-run both notebooks in order to regenerate all four files below from their raw sources.

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
| `n_northern_with_agreement` | Of those reporting, how many show `level >= 1`. **Not a clean "breadth of Northern integration" measure as-is** — see caveat below. |

**⚠️ EU multiplicity caveat, affecting `n_northern_reporting` and `n_northern_with_agreement` (but *not* `depth_score`/`reciprocal`/`fta_or_deeper`/`nonreciprocal_only`):** 23 of the 36 "Northern partners" in this panel are EU member states, and Baier–Bergstrand codes the EU's single common trade policy as 23 separate, identical bilateral rows rather than one "EU" relationship. A country whose only real Northern relationship is with the EU as a bloc will show `n_northern_with_agreement` around 23-plus, which reads as broad multi-partner integration but is actually one policy relationship counted 23 times. `depth_score` and everything derived from it are unaffected (a maximum over 23 identical values is the same as a maximum over one) — this only distorts columns that *count* partners. Collapse the EU's members to a single partner before using either count column as a genuine breadth measure, or skip straight to bilateral trade-value weighting (methodology doc §6.2 step 5), which sidesteps the issue rather than needing a separate fix.

**Diagnostics run on this panel and confirmed in the notebook:** `reciprocal` never reverts from 1 to 0 for any of the 24 countries over 1950–2017 (empirically absorbing); `nonreciprocal_only` does show 17 apparent reversions, but every one is a graduation to reciprocal status, not a genuine loss of preference, within this specific country set and window.

## `baier_bergstrand_africa_northern_bilateral.csv`

The intermediate bilateral panel this is built from: one row per (African country, Northern partner, year) — 24 × 36 × up to 68 years. Kept for future extensions that need partner-level detail (e.g., trade- or GDP-weighted exposure, per methodology doc §6.2 step 5, once bilateral trade-flow data is added).

| Column | Meaning |
|---|---|
| `african_iso3`, `northern_iso3` | ISO3 codes |
| `year` | 1950–2017 |
| `level` | Baier–Bergstrand integration level (0–6), collapsed from the two directional rows by taking the max — see the notebook for why this is the economically correct collapse rule for this project (non-reciprocal preferences are directional; reciprocal levels already agree in both directions) |
| `not_yet_country` | Whether either direction was `NoCty` |

## `trade_agreements_with_exposure_country_year.csv`

`baier_bergstrand_africa_northern_country_year.csv` (all columns above) plus the trade- and GDP-weighted exposure variables built in `notebooks/02_clean_cepii_gravity.ipynb` from the CEPII Gravity dataset. This is the more complete of the two country–year files — prefer it over the Baier–Bergstrand-only version for any analysis that wants the weighted exposure measures.

| Column | Meaning |
|---|---|
| `trade_total_northern` | Total bilateral trade (IMF DOTS, mirror-averaged, both directions summed) with all 36 Northern partners that year, in thousands of current USD. `NaN` if no partner has any trade data that year (a true missing-data case), not `0`. |
| `trade_reciprocal_northern` | Of that total, the portion with partners at `reciprocal` (level ≥ 2) status. Correctly `0` (not `NaN`) when zero partners are reciprocal that year — see the caveat below. |
| `reciprocal_trade_share` | `trade_reciprocal_northern / trade_total_northern` — the trade-weighted exposure measure, in `[0, 1]`. ~91.4% non-null (missing where `trade_total_northern` is missing). |
| `gdp_total_northern`, `gdp_reciprocal_northern`, `reciprocal_gdp_share` | Same construction, weighted by Northern partners' GDP instead of bilateral trade. 100% non-null — prefer this measure where full-sample coverage matters more than trade-value precision. |
| `n_reciprocal_partners`, `n_partners_trade_available` | Diagnostic counts: how many Northern partners were at reciprocal status, and how many had non-missing trade data, that year. |

**Why weighting was needed at all, and why it sidesteps the EU-multiplicity caveat above:** `reciprocal` and `depth_score` both take a *maximum* across partners, so they can't distinguish an agreement with an economically massive partner (the EU) from one with a negligible partner (Malta alone), and a naive partner *count* is structurally inflated by the EU's 23 member states being coded as 23 identical rows. Weighting by real bilateral trade value (or GDP) fixes both at once: each EU member still contributes its own real, distinct trade/GDP figure, so there is no double-counting to correct for — see methodology doc §6.2 step 5.

**A data-quality note carried over from the notebook**: CEPII's `country_exists` flag is slightly stricter than Baier–Bergstrand's own coding for seven Northern partners in their pre-independence years (Czechoslovakia's 1993 split, the Baltic states' and Slovenia's 1991 independence, Singapore's 1965 independence) — this only narrows the weighting denominators in those specific partner-years, and does not affect `reciprocal`/`depth_score`/`fta_or_deeper`/`nonreciprocal_only`, which come entirely from `baier_bergstrand_africa_northern_country_year.csv`.

**Own-country control variables** (added for the future regression stage — methodology doc §6.5 — rather than for the exposure calculation itself):

| Column | Meaning |
|---|---|
| `gdp_african`, `gdpcap_african` | The African country's own GDP and GDP per capita, current thousands USD (WDI, backfilled with Barbieri for years before WDI's 1960 start). ~81% non-null across 1950–2017. |
| `gdp_ppp_african`, `gdpcap_ppp_african` | PPP-adjusted versions of the same (current international $, WDI only — not backfilled). Only ~41% non-null; prefer the non-PPP versions above for full-sample work. |
| `pop_african` | Population, thousands (WDI, backfilled with Maddison). 100% non-null. |
| `pop_pwt_african`, `gdp_ppp_pwt_african` | Alternate-source (Penn World Table) population and PPP GDP — a cross-check against the WDI-based versions above, not a preferred default. ~87% non-null. |
| `wto_african`, `gatt_african` | 1/0 WTO and GATT membership dummies. 100% non-null; validated against South Africa's known 1995 WTO accession (switches 0→1 exactly that year) and its GATT membership since 1948 (constant 1 throughout). |

None of these were checked cell-by-cell the way the trade/exposure variables were (see the notebook's own §12 caveat) — spot-check before leaning on them heavily in a regression.

## `cepii_gravity_africa_northern_bilateral.csv`

The intermediate bilateral panel `trade_agreements_with_exposure_country_year.csv` is built from: one row per (African country, Northern partner, year), 52,392 rows (fewer than the Baier–Bergstrand bilateral file's 58,752 — see the existence-flag note above). Kept for future extensions that need partner-level trade/GDP detail.

| Column | Meaning |
|---|---|
| `african_iso3`, `northern_iso3`, `year` | As above |
| `trade_imf` | Bilateral trade (IMF DOTS, mirror-averaged across the two directional rows, both directions summed), thousands of current USD. `NaN` (not `0`) where no direction has data. |
| `trade_imf_n_directions` | 0, 1, or 2 — how many of the two directional flows contributed a non-missing value. |
| `gdp_northern`, `gdp_ppp_northern` | The Northern partner's own GDP that year (current USD and PPP-adjusted). |
