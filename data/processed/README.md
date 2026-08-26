# Processed data

Outputs of four notebooks:

1. [`notebooks/01_clean_trade_agreements.ipynb`](../../notebooks/01_clean_trade_agreements.ipynb) — builds the African–Northern trade-agreement panel described in `docs/developing-country-trade-productivity.md` §5–§6, from the raw source in `data/baier-bergstrand/`.
2. [`notebooks/02_clean_cepii_gravity.ipynb`](../../notebooks/02_clean_cepii_gravity.ipynb) — builds the trade- and GDP-weighted exposure variables (methodology doc §6.2 step 5), from the raw source in `data/cepii-gravity/`, and merges them onto notebook 1's country–year panel. Run after notebook 1.
3. [`notebooks/03_clean_ggdc_productivity.ipynb`](../../notebooks/03_clean_ggdc_productivity.ipynb) — aggregates the 9-sub-sector GGDC productivity panel down to Agriculture/Manufacturing/Services, from the raw source `data/Global-Productivity-Sectoral-Database.dta`, then runs the McMillan & Rodrik (2011) productivity decomposition on the result (methodology doc §6.3). Independent of notebooks 1–2 (different source, not yet merged with the trade-agreement panel).
4. [`notebooks/04_build_estimation_panel.ipynb`](../../notebooks/04_build_estimation_panel.ipynb) — merges notebook 3's decomposition output onto notebook 2's trade-agreement panel, producing the analysis-ready dataset methodology doc §6.4's regressions run on. Run after notebooks 1–3.
5. [`notebooks/05_descriptive_event_study.ipynb`](../../notebooks/05_descriptive_event_study.ipynb) — the descriptive event-study plots and other descriptives methodology doc §6.4.A calls for, before any regression. Uses the annual (not interval) panels from notebooks 1 and 3, so it can run any time after those two.

Re-run in order (1, then 2, then 4; 3 can run independently of 1–2 but must run before 4) to regenerate the nine files below from their raw sources.

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

## `ggdc_africa_broad_sectors.csv`

The GGDC sectoral productivity panel (`data/Global-Productivity-Sectoral-Database.dta`), restricted to the project's 24 African countries and aggregated from its native 9 sub-sectors down to the three broad sectors — Agriculture, Manufacturing, Services — that the McMillan–Rodrik decomposition (methodology doc §6.3) runs on. One row per (African country, broad sector, year), 1950–2017 (coverage varies sharply by country — see below).

**The aggregation follows Herrendorf, Rogerson & Valentinyi (2014), "Growth and Structural Transformation"** (NBER WP 18996 / Handbook of Economic Growth Vol. 2B Ch. 6), confirmed directly from their Data Appendix (pp. 95–96) for the specific data source this project uses (the Groningen/GGDC 10-sector database, their own primary historical source):

| Broad sector | GGDC sub-sectors mapped in |
|---|---|
| **Agriculture** | `1.Agriculture` |
| **Manufacturing** | `2.Mining`, `3.Manufacturing`, `5.Construction` |
| **Services** | `4.Utilities`, `6.Trade services`, `7.Transport services`, `8.Finance amd business services` [sic, source typo], `9.Other services` |

The one detail worth flagging because it's easy to guess wrong: **utilities go with Services, not Manufacturing**, under HRV's own ISIC-code-based rule for this specific source — their paper uses a different rule (utilities → industry) for some of their *other* non-GGDC data sources, but this is the one tied to the actual database used here.

| Column | Meaning |
|---|---|
| `iso3`, `country`, `year` | As elsewhere; `country` keeps the source's own name string (e.g. "United Republic of Tanzania" for `TZA`) |
| `broad_sector` | `Agriculture`, `Manufacturing`, or `Services` |
| `value_added_real` | Real value added, summed across the mapped sub-sectors, in the source's native units (not independently re-verified — see the notebook's own caveat) |
| `employment` | Employment, summed across the mapped sub-sectors |
| `employment_share` | This broad sector's share of the country-year's total employment across all three broad sectors; sums to exactly 1.0 within a country-year wherever no sub-sector is missing |
| `labor_productivity_real` | Recomputed as aggregated `value_added_real` / aggregated `employment` — **not** an average of the sub-sectors' own productivity figures, which would be the wrong aggregation |
| `labor_productivity_ppp` | Same idea, but for the PPP-based measure, which required backing out an implied PPP value-added series first (`source productivity × source employment`) since the raw source only reports the ratio, not its PPP-terms numerator — validated to match the source's own `Total` row exactly (float32 precision, ~1e-7 relative) |

**Data-quality notes**: Namibia (1960–1964) and Botswana (1964–1967) have a handful of country-years with at least one missing sub-sector, which propagate as genuine `NaN` (not silently `0`) in the aggregated output. Country coverage starts as early as 1960 for some countries (Ghana, Egypt, Nigeria, South Africa, Tanzania) but as late as 1999–2001 for others (Algeria, Sierra Leone) — a real data-availability constraint, not a cleaning artifact. `Value_added_nominal` from the raw source was not carried forward — only what was asked for (employment shares, real value added, labor productivity).

**Not yet merged** with `trade_agreements_with_exposure_country_year.csv` — that's the next step (methodology doc §6.2 step 6), joining on `iso3`/`year`.

## McMillan–Rodrik decomposition files

The McMillan & Rodrik (2011) productivity decomposition (methodology doc §6.3), run on `ggdc_africa_broad_sectors.csv` in `notebooks/03_clean_ggdc_productivity.ipynb` §10–§14. Confirmed directly from the paper's own equation (1) — this caught and fixed a timing error in an earlier version of the methodology doc, which had the structural-change term using beginning-of-period rather than end-of-period productivity levels.

```
ΔY_t = Σ_i θ_i,(t-k) · Δy_i,t   +   Σ_i y_i,t · Δθ_i,t
```

`θ` is employment share, `y` is labor productivity (here, `labor_productivity_real`), and the two terms sum to the *actual* change in economy-wide productivity with zero residual by construction — verified numerically (residuals of order 1e-12 to 1e-13, not merely small).

**Eight files**, splitting three independent choices — country-level total vs. broken out by sector; and four interval schemes (annual, full-period, 3-year, 5-year):

| File | Grain | `year0`/`year1` |
|---|---|---|
| `mcmillan_rodrik_decomposition_annual.csv` | One row per (country, consecutive year pair) — the three sectors already summed | Always one calendar year apart |
| `mcmillan_rodrik_decomposition_annual_by_sector.csv` | One row per (country, consecutive year pair, broad sector) | Always one calendar year apart |
| `mcmillan_rodrik_decomposition_fullperiod.csv` | One row per country — the three sectors already summed | That country's first and last years with complete data for all three broad sectors (varies by country — e.g. 1960–2017 for Ghana, 2002–2017 for Angola) |
| `mcmillan_rodrik_decomposition_fullperiod_by_sector.csv` | One row per (country, broad sector) | Same window as the country-level file above |
| `mcmillan_rodrik_decomposition_interval3.csv` | One row per (country, 3-year bin) — sectors summed | Fixed calendar grid shared across all countries, anchored at 1960 (e.g. 1960–1963, 1963–1966, ...) |
| `mcmillan_rodrik_decomposition_interval3_by_sector.csv` | One row per (country, 3-year bin, broad sector) | Same grid |
| `mcmillan_rodrik_decomposition_interval5.csv` | One row per (country, 5-year bin) — sectors summed | Fixed calendar grid shared across all countries, anchored at 1960 (e.g. 1960–1965, 1965–1970, ...) |
| `mcmillan_rodrik_decomposition_interval5_by_sector.csv` | One row per (country, 5-year bin, broad sector) | Same grid |

The `_by_sector` files' `within`/`structural_change` for a given (country, year0, year1) sum *exactly* (to ~1e-13) to the corresponding row in the non-`_by_sector` file — verified in the notebook, not assumed. **The sector-level files are the more useful pair for this project's actual research question** (the impact of trade agreements on *sectoral* productivity) — the country-level totals collapse away exactly the sector detail the theoretical channels in methodology doc §2.2–§2.3 make predictions about.

**The `interval3`/`interval5` files are the intended input for the eventual staggered-DiD estimation, not `annual` or `fullperiod`.** Annual differences are too noisy for a regression outcome (both components are first differences of already-noisy series, and structural change specifically is a slow process that's mostly noise at one-year resolution); full-period leaves only one row per country, no panel structure for a staggered estimator to use. The interval files use a **shared calendar grid** — fixed-width bins applied identically to every country (not bins anchored to each country's own start year, which would make "period 1" mean different calendar years for different countries and break year-comparability) — anchored at 1960, the panel's own earliest year. A convenient property of that anchor: the 3-year grid (`1960 + 19×3 = 2017`) lands exactly on the panel's last year for every full-coverage country, so nothing is lost at the tail; the 5-year grid doesn't divide evenly, so full-coverage countries lose their last 2 years (2015–2017) to no bin at 5-year resolution.

**Both widths were kept deliberately, not just one** — 5-year smooths out more noise (favored on that basis), 3-year preserves more periods for countries with short coverage, where 5-year bites hardest. Checked directly rather than assumed: at 3-year resolution, every one of the 24 countries has at least 5 bins. At 5-year resolution, only **2 of 24 — Angola and Sierra Leone — fall below 3 bins** (both have exactly 2). Every interval file carries a `short_series` column (`True` for those 2 countries at 5-year resolution; nobody is flagged at 3-year) rather than silently dropping them. Given how few are affected, the recommendation is to **keep them by default** and treat exclusion as a robustness check rather than a baseline choice — a staggered panel estimator pools identifying variation across all units via its fixed effects, so a thin country contributes less precision, not zero information, and both Angola and Sierra Leone are resource-dependent economies directly relevant to the resource-dependence heterogeneity cut already planned in methodology doc §6.4.D. Dropping them outright would remove exactly the cases that cut is meant to test.

| Column | Meaning |
|---|---|
| `iso3`, `year0`, `year1` | As above |
| `broad_sector` (`_by_sector` files only) | `Agriculture`, `Manufacturing`, or `Services` |
| `Y0`, `Y1` (non-`_by_sector` files only) | Economy-wide labor productivity (employment-share-weighted average across the three broad sectors) at `year0` and `year1` |
| `employment_share_0`/`_1`, `productivity_0`/`_1` (`_by_sector` files only) | That sector's employment share and `labor_productivity_real` at `year0`/`year1` — the raw inputs each row's `within`/`structural_change` is built from |
| `within` | The within-sector component: `θ_i,(t-k) · Δy_i,t` (sector-level) or `Σ_i θ_i,(t-k) · Δy_i,t` (country-level) |
| `structural_change` | The structural-change (reallocation) component: `y_i,t · Δθ_i,t` (sector-level) or `Σ_i y_i,t · Δθ_i,t` (country-level) |
| `actual_change`, `decomposed_change` (non-`_by_sector` files only) | `Y1 - Y0`, and `within + structural_change` — identical to floating-point precision; kept as separate columns so this is checkable rather than assumed |
| `pct_within`, `pct_structural_change` (`mcmillan_rodrik_decomposition_fullperiod.csv` only) | Each component's share of `decomposed_change`. **Can be far outside [0, 100]%** when the two components partly offset (e.g. Malawi: -370%/+470%, because its net change is small relative to both components) — a real feature of shift-share decompositions when net change is near zero, not a bug. |
| `short_series` (`interval3`/`interval5` files only) | `True` if that country has fewer than 3 bins at that file's interval width. Kept as a flag, not acted on — nobody is excluded from these files on this basis. |

**Qualitative validation against the literature** (methodology doc §2.6): country-level within-sector is positive and the larger component for most countries; structural change is negative for 4 of 24 countries (Zambia, Eswatini, Sierra Leone, Angola) — consistent with, though not a strict replication of, McMillan & Rodrik (2011) and Diao, McMillan & Rodrik's "African paradox" finding that structural change in Africa is often growth-reducing rather than growth-enhancing. At the sector level, a clean textbook structural-transformation pattern shows up (e.g. Ghana 1960–2017): agriculture's employment share falls (a *negative* structural-change contribution, `-1.40`), while manufacturing's and especially services' shares rise (`+0.35`, `+2.11`) — labor moving out of agriculture and into manufacturing/services. Not a specific published number to match — this project's sample period and country set both differ from the original papers'.

**Now merged with `trade_agreements_with_exposure_country_year.csv`** — see `estimation_panel_interval3.csv` / `estimation_panel_interval5.csv` below, built by `notebooks/04_build_estimation_panel.ipynb`.

## `estimation_panel_interval3.csv` and `estimation_panel_interval5.csv`

**The dataset methodology doc §6.4's regressions are meant to run on.** Built by [`notebooks/04_build_estimation_panel.ipynb`](../../notebooks/04_build_estimation_panel.ipynb), merging the `mcmillan_rodrik_decomposition_interval{3,5}[_by_sector].csv` files above onto `trade_agreements_with_exposure_country_year.csv`. One row per (country, interval, sector-or-`"Total"`) — the country-level totals from the non-`_by_sector` decomposition file are folded in as a `broad_sector == "Total"` pseudo-sector row (employment share trivially 1.0, productivity = economy-wide productivity), the same convention the raw GGDC source itself uses for its own `Total` row alongside its sub-sectors — so this single file serves both the sector-level primary analysis and the country-level companion summary; filter on `broad_sector` to pick one or the other.

| Column | Meaning |
|---|---|
| `iso3`, `country`, `year0`, `year1`, `broad_sector` | As in the source decomposition files, plus `country` (looked up, since the decomposition files only carry `iso3`) |
| `within`, `structural_change`, `employment_share_0`/`_1`, `productivity_0`/`_1`, `short_series` | Carried over unchanged from the sector-level decomposition file (or synthesized for `"Total"` rows as described above) |
| `country_productivity_0`/`_1`, `country_actual_change`, `country_decomposed_change` | The country-level context for this row's interval, **explicitly prefixed** so a sector-level row's own values (`within`, `structural_change`, `productivity_0`/`_1`) aren't mistaken for the country total — these four columns repeat identically across all four `broad_sector` rows in a group |
| `*_pre` columns (`country_exists_pre`, `depth_score_pre`, `agreement_type_pre`, `reciprocal_pre`, `fta_or_deeper_pre`, `nonreciprocal_only_pre`, `reciprocal_trade_share_pre`, `reciprocal_gdp_share_pre`, `gdp_african_pre`, `gdpcap_african_pre`, `pop_african_pre`, `wto_african_pre`, `gatt_african_pre`) | Trade-agreement-side variables at `year0` — **deliberately pre-determined** (start-of-interval), not `year1` or an average, to avoid "bad control" bias from controls the treatment could itself have affected |
| `reciprocal_post`, `country_exists_post` | `reciprocal` and `country_exists` at `year1`, used only to derive `treatment_status` below |
| `treatment_status` | `never_reciprocal`, `always_reciprocal`, `switched_during_interval`, or `missing_reciprocal_data` (country didn't yet exist at `year0` or `year1` — e.g. Tanzania's 1960–1963/1963–1966 intervals, pre-1964 union). A fifth logical case, a reversion (`reciprocal_pre=1` and `reciprocal_post=0`), is checked for and confirmed to never occur — `Reciprocal_c,t`'s absorbing property (confirmed annually in notebook 01) holds at interval resolution too. |
| `reciprocal_interval` | The simplest usable binary treatment indicator: 1 if `treatment_status` is `always_reciprocal` or `switched_during_interval`, else 0. **`switched_during_interval` bins mix pre- and post-treatment dynamics within the same bin** — decide deliberately (drop, or treat as its own category) how to handle them in a regression rather than pooling them in unexamined; `treatment_status` is kept as its own column precisely so this choice isn't made silently. |

**Validated against South Africa's 2000 EU TDCA entry**: the 1996–1999 interval shows `never_reciprocal`, 1999–2002 (which contains the 2000 entry date) shows `switched_during_interval`, and 2002–2005 shows `always_reciprocal` — the classification lines up with real history, not just internal consistency.

Only a subset of the trade panel's own columns were carried over as `_pre` controls — the fuller set (Northern-partner counts, raw trade/GDP totals) remains in `trade_agreements_with_exposure_country_year.csv` if needed later. The `short_series` flag (Angola, Sierra Leone at 5-year resolution) is inherited as-is, still not excluded — see the recommendation in methodology doc §6.3 to keep them in the baseline sample.

## Descriptive event-study outputs

Small summary tables from [`notebooks/05_descriptive_event_study.ipynb`](../../notebooks/05_descriptive_event_study.ipynb) (methodology doc §6.4.A) — the underlying charts themselves live in the notebook, these are the aggregated data behind them, kept for reuse without re-running the notebook.

| File | Contents |
|---|---|
| `descriptive_treatment_adoption.csv` | One row per country: `first_reciprocal_year` (blank for the 7 never-treated countries) and `ever_treated` (0/1). |
| `descriptive_event_study_productivity.csv` | Mean/SEM/count of the log productivity index (indexed to 0 at event time -1) by `broad_sector` and `event_time`, event time in [-10, +7]. Algeria excluded (no pre-treatment data in this project's GGDC coverage — treated in 1976 per Baier–Bergstrand, but GGDC data only starts 1999). |
| `descriptive_event_study_decomposition.csv` | Mean/SEM of the annual `within`/`structural_change` decomposition components by `broad_sector` and `event_time` (two-level column header: variable, then statistic). |

**Headline findings** (full discussion, including what's a genuine pattern vs. likely small-sample noise, is in the notebook itself):

- Treatment adoption clusters in two cohorts (1976–2000, then 2007–2010) rather than spreading smoothly — any pooled event-time average over-represents the larger, later cohort at long horizons.
- Productivity *levels* show a pre-existing rising trend in all three sectors that **accelerates** post-treatment in Agriculture (4.4x steeper) and Services (2.6x steeper) but **decelerates** in Manufacturing (to 0.3x its pre-treatment slope) — plausibly consistent with short-run adjustment costs from import competition (methodology doc §2.2–§2.3), not necessarily evidence against the design.
- The annual within-sector *growth rate* (the actual regression outcome, not the level) shifts from negative to positive after treatment in Manufacturing and Services specifically — the sectors and direction the theoretical channels predict.
- Never-treated countries have similar median growth to treated countries in every sector (supporting their use as a comparison group) but far more dispersion (IQR 5–7x wider in Agriculture/Manufacturing) — a source of noisier standard errors, not bias, at the regression stage.
- One country (Algeria) has zero pre-treatment observations in this project's data and cannot be event-time-aligned at all.
