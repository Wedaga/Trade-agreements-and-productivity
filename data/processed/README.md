# Processed data

Outputs of four notebooks:

1. [`notebooks/01_clean_trade_agreements.ipynb`](../../notebooks/01_clean_trade_agreements.ipynb) — builds the African–Northern trade-agreement panel described in `docs/developing-country-trade-productivity.md` §5–§6, from the raw source in `data/baier-bergstrand/`.
2. [`notebooks/02_clean_cepii_gravity.ipynb`](../../notebooks/02_clean_cepii_gravity.ipynb) — builds the trade- and GDP-weighted exposure variables (methodology doc §6.2 step 5), from the raw source in `data/cepii-gravity/`, and merges them onto notebook 1's country–year panel. Run after notebook 1.
3. [`notebooks/03_clean_ggdc_productivity.ipynb`](../../notebooks/03_clean_ggdc_productivity.ipynb) — aggregates the 9-sub-sector GGDC productivity panel down to Agriculture/Manufacturing/Services, from the raw source `data/Global-Productivity-Sectoral-Database.dta`, then runs the McMillan & Rodrik (2011) productivity decomposition on the result (methodology doc §6.3). Independent of notebooks 1–2 (different source, not yet merged with the trade-agreement panel).
4. [`notebooks/04_build_estimation_panel.ipynb`](../../notebooks/04_build_estimation_panel.ipynb) — merges notebook 3's decomposition output onto notebook 2's trade-agreement panel, producing the analysis-ready dataset methodology doc §6.4's regressions run on. Run after notebooks 1–3.
5. [`notebooks/05_descriptive_event_study.ipynb`](../../notebooks/05_descriptive_event_study.ipynb) — the descriptive event-study plots and other descriptives methodology doc §6.4.A calls for, before any regression. Uses the annual (not interval) panels from notebooks 1 and 3, so it can run any time after those two.
6. [`notebooks/06_twfe_baseline.ipynb`](../../notebooks/06_twfe_baseline.ipynb) — the baseline two-way fixed-effects regressions methodology doc §6.4.B and §6.3 Design A specify, run as a comparison point only (§2.8's bias warning). Uses notebooks 1/3's annual panels plus notebook 4's interval estimation panels; run after notebook 4.

Re-run in order (1, then 2, then 4; 3 can run independently of 1–2 but must run before 4 and 6; 5 needs 1 and 3; 6 needs 1, 3, and 4) to regenerate the ten files below from their raw sources.

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

**Data-quality notes**: Namibia (1960–1964) and Botswana (1964–1967) have a handful of country-years with at least one missing Services sub-sector. **Correction to a claim made here previously, caught while building `ggdc_africa_subsectors.csv` (below)**: this used to say the affected cells come out as genuine `NaN` rather than silently `0`. Checked directly against this file itself: that's not actually true for 9 of these country-years (Namibia 1960–1964, Botswana 1964–1967) — Services' `value_added_real` is a literal `0.0`, not `NaN`, because exactly 1 of Services' 5 sub-sectors (`"9.Other services"`) reports `Value_added_real = 0` in the raw source (itself a data artifact, not a genuine zero) while the other 4 are properly missing, and the `sum(min_count=1)` aggregation treats "1 present, 4 missing" as complete rather than partial — so this file's own Services productivity is silently understated (not `NaN`) for those 9 country-years. Not fixed here (this file is left exactly as it always was); `ggdc_africa_subsectors.csv` applies a `0 → NaN` correction to that one sub-sector specifically, so anything built from it (the interval decomposition files below) doesn't inherit the issue — a fix to this file itself would be a good follow-up. Country coverage starts as early as 1960 for some countries (Ghana, Egypt, Nigeria, South Africa, Tanzania) but as late as 1999–2001 for others (Algeria, Sierra Leone) — a real data-availability constraint, not a cleaning artifact. `Value_added_nominal` from the raw source was not carried forward — only what was asked for (employment shares, real value added, labor productivity).

**Not yet merged** with `trade_agreements_with_exposure_country_year.csv` — that's the next step (methodology doc §6.2 step 6), joining on `iso3`/`year`.

## `ggdc_africa_subsectors.csv`

GGDC's own 9 raw sub-sectors (not collapsed to the 3 broad sectors above), built in `notebooks/03_clean_ggdc_productivity.ipynb` §9a specifically to feed the interval decomposition below — the annual/full-period decomposition doesn't need it and still runs on `ggdc_africa_broad_sectors.csv` directly.

| Column | Meaning |
|---|---|
| `iso3`, `country`, `year` | As elsewhere |
| `sector` | One of GGDC's 9 raw sub-sectors (verbatim source labels, including its own `"8.Finance amd business services"` typo) |
| `broad_sector` | Which of Agriculture/Manufacturing/Services this sub-sector maps to (Herrendorf–Rogerson–Valentinyi, same mapping as `ggdc_africa_broad_sectors.csv`) — Agriculture has 1 sub-sector, Manufacturing 3 (Mining, Manufacturing, Construction), Services 5 (Utilities, Trade, Transport, Finance and business, Other services) |
| `value_added_real`, `employment` | That sub-sector's own value added and employment. **One data-quality fix applied here that is *not* applied to `ggdc_africa_broad_sectors.csv`**: `"9.Other services"` reporting `Value_added_real = 0` (a literal source-data zero, not `NaN`) for Namibia 1960–1964 and Botswana 1964–1967 — see the note on `ggdc_africa_broad_sectors.csv` above — is treated as missing (`NaN`) here, so it doesn't silently masquerade as valid data in anything built from this file |
| `employment_country` | That country-year's total employment summed across all 9 sub-sectors — validated against the source's own `Total` row (~1e-7 relative error, float32 precision) |
| `y` | That sub-sector's own productivity, `value_added_real / employment` |
| `l` | That sub-sector's employment share of the **country's** total employment, `employment / employment_country` — **not** its share of its own broad sector's employment, a deliberate choice the interval decomposition below depends on |

## McMillan–Rodrik decomposition files

The McMillan & Rodrik (2011) productivity decomposition (methodology doc §6.3), run in `notebooks/03_clean_ggdc_productivity.ipynb` §10–§14. Confirmed directly from the paper's own equation (1) — this caught and fixed a timing error in an earlier version of the methodology doc, which had the structural-change term using beginning-of-period rather than end-of-period productivity levels.

```
ΔY_t = Σ_i θ_i,(t-k) · Δy_i,t   +   Σ_i y_i,t · Δθ_i,t
```

`θ` is employment share, `y` is labor productivity, and the two terms sum to the *actual* change in economy-wide productivity with zero residual by construction — verified numerically (residuals of order 1e-12 to 1e-13, not merely small). **The annual and full-period files implement this equation directly**, on `ggdc_africa_broad_sectors.csv`, with the 3 broad sectors themselves as the index `i` and `y`/`θ` as single-year point-in-time snapshots (`labor_productivity_real`/`employment_share`) at `year0`/`year1`.

**The `interval3`/`interval5` files follow a different, nested version of the same equation instead — [`docs/decomposition.tex`](../../docs/decomposition.tex) equation (3), the growth-rate form, applied separately within each broad sector.** Rather than treating the 3 broad sectors as the index, each broad sector *b*'s own within/structural-change split is built from GGDC's finer 9 sub-sectors (`ggdc_africa_subsectors.csv` above) — Agriculture from its 1, Manufacturing from 3, Services from 5 — with sub-sector employment shares `l_i = L_i/L_country` normalized by **country-total** employment, not each sub-sector's share of its own broad sector. The quantity actually being decomposed, `LP_b = Σ_{i∈b} y_i·l_i`, works out to `Q_b/L_country` — *sector b's own contribution to country-wide productivity* — not `Q_b/L_b` (sector b's own standalone productivity, which is a separate, still-available quantity — see the column table below). `within`/`structural_change` are then the level-form terms **divided by `LP_b` at the start of the interval** — small, unitless growth-rate contributions (typically a few percentage points), not raw productivity-level changes. This is a deliberate, substantive redesign (not a refinement) of what these two files used to contain — level-form, unnormalized changes with the 3 broad sectors pooled as the index, matching the annual/full-period files' own approach.

**Ten files** now, not eight — the two decomposition schemes above (annual/full-period vs. interval) plus `ggdc_africa_subsectors.csv`:

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

**Two "sums to total" identities hold for the interval files, not one — and only in a specific form, demonstrated directly in the notebook rather than assumed.** The **level-form** terms (`within_level`, `structural_change_level`, before growth-rate division) sum *exactly* across the 3 broad sectors to the country-level file's own level-form terms (max gap ~1e-14). The **growth-rate** terms (`within`, `structural_change`, what's actually in these columns) do **not** sum the same way — each broad sector normalizes by its own `LP_b` at the start of the interval, the country file normalizes by the country's own `LP_country`, different denominators — a nonzero, non-bug gap (median ~0.13 in one demonstration). This replaces a simpler identity the annual/full-period files still have (their `_by_sector` `within`/`structural_change` sum exactly to the non-`_by_sector` file, since both use the same undivided, single-normalization-base level form).

The interval files' completeness requirement is now **per calendar-grid edge year, but against all 9 sub-sectors** (not just the 3 broad sectors, as the annual/full-period files require) — a stricter bar than the annual files', though not as strict as an earlier, now-superseded period-averaging version of this pipeline that additionally required every year *inside* each window to be complete, not just its two edges.

**The `interval3`/`interval5` files are the intended input for the eventual staggered-DiD estimation, not `annual` or `fullperiod`.** Annual differences are too noisy for a regression outcome (both components are first differences of already-noisy series, and structural change specifically is a slow process that's mostly noise at one-year resolution); full-period leaves only one row per country, no panel structure for a staggered estimator to use. The interval files use a **shared calendar grid** — fixed-width bins applied identically to every country (not bins anchored to each country's own start year, which would make "period 1" mean different calendar years for different countries and break year-comparability) — anchored at 1960, the panel's own earliest year.

**Both widths were kept deliberately, not just one** — 5-year smooths out more noise (favored on that basis), 3-year preserves more periods for countries with short coverage, where 5-year bites hardest. Checked directly: at 3-year resolution every one of the 24 countries has at least 5 bins; at 5-year resolution, **2 of 24 — Angola and Sierra Leone — fall below 3 bins**. Every interval file carries a `short_series` column (`True` for those 2 countries at 5-year resolution; nobody is flagged at 3-year) rather than silently dropping them. Given how few are affected, the recommendation is to **keep them by default** and treat exclusion as a robustness check rather than a baseline choice — a staggered panel estimator pools identifying variation across all units via its fixed effects, so a thin country contributes less precision, not zero information, and both are resource-dependent economies directly relevant to the resource-dependence heterogeneity cut already planned in methodology doc §6.4.D. Dropping them outright would remove exactly the cases that cut is meant to test.

| Column | Meaning |
|---|---|
| `iso3`, `year0`, `year1` | As above |
| `broad_sector` (`_by_sector` files only) | `Agriculture`, `Manufacturing`, or `Services` |
| `Y0`, `Y1` (non-`_by_sector` files only) | Economy-wide labor productivity (employment-share-weighted average across the three broad sectors) at `year0` and `year1` |
| `employment_share_0`/`_1`, `productivity_0`/`_1` (`_by_sector` files only) | That sector's own **standalone** employment share and labor productivity (`L_b/L_country` and `Q_b/L_b`) at `year0`/`year1`, unchanged in meaning across every file (annual, full-period, and interval alike) — point-in-time at the two years, not averaged |
| `contribution_productivity_0`/`_1` (`interval3`/`interval5` `_by_sector` files only) | `LP_b` — that sector's *contribution* to country-wide productivity (`Q_b/L_country`, **not** the same quantity as `productivity_0`/`_1`) — the base the growth-rate `within`/`structural_change` below are normalized by |
| `within_level`, `structural_change_level` (`interval3`/`interval5` files only) | The pre-normalization level-form terms — kept for transparency and because the exact "sums to total" identity only holds at this level, not after growth-rate division |
| `within` | The within-sector growth-rate contribution. Annual/full-period: `θ_i,(t-k)·Δy_i,t` (sector-level, level form) or `Σ_i θ_i,(t-k)·Δy_i,t` (country-level). Interval: `within_level / LP_b,0` (sector-level) or `within_level(country) / LP_country,0` (country-level) — see above |
| `structural_change` | The structural-change (reallocation) component, same annual-vs-interval distinction as `within` above |
| `actual_change`, `decomposed_change` (non-`_by_sector` files only) | Annual/full-period: `Y1 - Y0` and `within + structural_change` (level form). Interval: `(Y1-Y0)/Y0` and `within + structural_change` (growth-rate form) — identical to floating-point precision either way; kept as separate columns so this is checkable rather than assumed |
| `pct_within`, `pct_structural_change` (`mcmillan_rodrik_decomposition_fullperiod.csv` only) | Each component's share of `decomposed_change`. **Can be far outside [0, 100]%** when the two components partly offset (e.g. Malawi: -370%/+470%, because its net change is small relative to both components) — a real feature of shift-share decompositions when net change is near zero, not a bug. |
| `short_series` (`interval3`/`interval5` files only) | `True` if that country has fewer than 3 bins at that file's interval width. Kept as a flag, not acted on — nobody is excluded from these files on this basis. |

**Qualitative validation against the literature** (methodology doc §2.6), from the **unaffected** annual/full-period files: country-level within-sector is positive and the larger component for most countries; structural change is negative for 4 of 24 countries (Zambia, Eswatini, Sierra Leone, Angola) — consistent with, though not a strict replication of, McMillan & Rodrik (2011) and Diao, McMillan & Rodrik's "African paradox" finding that structural change in Africa is often growth-reducing rather than growth-enhancing. At the sector level, a clean textbook structural-transformation pattern shows up (e.g. Ghana 1960–2017): agriculture's employment share falls (a *negative* structural-change contribution, `-1.40`), while manufacturing's and especially services' shares rise (`+0.35`, `+2.11`) — labor moving out of agriculture and into manufacturing/services. Not a specific published number to match — this project's sample period and country set both differ from the original papers'.

**Now merged with `trade_agreements_with_exposure_country_year.csv`** — see `estimation_panel_interval3.csv` / `estimation_panel_interval5.csv` below, built by `notebooks/04_build_estimation_panel.ipynb`.

## `estimation_panel_interval3.csv` and `estimation_panel_interval5.csv`

**The dataset methodology doc §6.4's regressions are meant to run on.** Built by [`notebooks/04_build_estimation_panel.ipynb`](../../notebooks/04_build_estimation_panel.ipynb), merging the `mcmillan_rodrik_decomposition_interval{3,5}[_by_sector].csv` files above onto `trade_agreements_with_exposure_country_year.csv`. One row per (country, interval, sector-or-`"Total"`) — the country-level totals from the non-`_by_sector` decomposition file are folded in as a `broad_sector == "Total"` pseudo-sector row (employment share trivially 1.0, productivity = economy-wide productivity), the same convention the raw GGDC source itself uses for its own `Total` row alongside its sub-sectors — so this single file serves both the sector-level primary analysis and the country-level companion summary; filter on `broad_sector` to pick one or the other. `contribution_productivity_0`/`_1` and `within_level`/`structural_change_level` (documented above) are **not** carried into this file — they exist purely as transparency/validation columns inside the decomposition files themselves.

| Column | Meaning |
|---|---|
| `iso3`, `country`, `year0`, `year1`, `broad_sector` | As in the source decomposition files, plus `country` (looked up, since the decomposition files only carry `iso3`) |
| `within`, `structural_change`, `employment_share_0`/`_1`, `productivity_0`/`_1`, `short_series` | Carried over unchanged from the sector-level decomposition file (or synthesized for `"Total"` rows as described above) — `within`/`structural_change` are growth-rate contributions (see above), `productivity_0`/`_1` are each sector's own standalone productivity level |
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
- Productivity *levels* show a pre-existing rising trend in all three sectors that **accelerates** post-treatment in Agriculture (4.4x steeper) and Services (2.6x steeper) but **decelerates** in Manufacturing (to 0.3x its pre-treatment slope) — plausibly consistent with short-run adjustment costs from import competition (methodology doc §2.2–§2.3), not necessarily evidence against the design. Individual-country trajectories (not just the pooled mean) confirm this is a genuine central tendency, not one or two outlier countries driving the average — countries are widely scattered around the mean in every sector, spanning more than a full log point.
- The annual within-sector *growth rate* (the actual regression outcome, not the level) shifts from negative to positive after treatment in Manufacturing and Services specifically — the sectors and direction the theoretical channels predict.
- Never-treated countries have similar median growth to treated countries in every sector (supporting their use as a comparison group) but far more dispersion (IQR 5–7x wider in Agriculture/Manufacturing) — a source of noisier standard errors, not bias, at the regression stage.
- One country (Algeria) has zero pre-treatment observations in this project's data and cannot be event-time-aligned at all.

## `twfe_baseline_results.csv`

The baseline two-way fixed-effects regressions from [`notebooks/06_twfe_baseline.ipynb`](../../notebooks/06_twfe_baseline.ipynb) (methodology doc §6.4.B and §6.3 Design A) — **a comparison point, not a credible causal estimate**; §2.8 documents why naive TWFE under staggered treatment timing is biased, and the staggered-adoption-robust estimator (§6.4.C, not yet built) is the primary specification. 18 rows: 2 outcomes (`within`, `structural_change`) × 2 interval widths × 2 levels (sector/country) × up to 3 sample variants (baseline, dropping `switched_during_interval` rows, dropping the `short_series` countries — 5-year sector-level only). Built on the nested, growth-rate-normalized interval decomposition values (`mcmillan_rodrik_decomposition_interval{3,5}_by_sector.csv` above, per `docs/decomposition.tex` eq. 3) — `within`/`structural_change` here are small growth-rate contributions, not raw productivity-level changes.

| Column | Meaning |
|---|---|
| `interval` | `interval3` or `interval5` |
| `level` | `sector` (Agriculture/Manufacturing/Services, `country x sector` and `sector x year` fixed effects) or `country` (the `"Total"` pseudo-sector rows, simple `country` and `year` fixed effects) |
| `variant` | `baseline (incl. switching)`, `drop switching intervals`, or `drop short-series countries` |
| `outcome` | `within` or `structural_change` |
| `beta`, `se`, `p` | The `reciprocal_interval` coefficient, its cluster-robust (by country) standard error, and p-value |
| `n`, `n_clusters` | Regression sample size and number of country clusters |

**A separate, single number** (not in this file — printed directly in the notebook, §4): the §6.4.B baseline itself, `ln(LaborProductivity_cst)` on the *annual* panel (not the interval panels this file covers) — β = 0.149, SE = 0.121, p = 0.22, N = 3,288.

**A genuine numerical issue, worth knowing before reusing this regression approach elsewhere in the project**: dummy-encoding `country x sector` and `sector x year` fixed effects together produces a design matrix that is *exactly* rank-deficient (by `n_sectors - 1`), because both fixed-effect sets involve the "sector" dimension. Left unfixed, the point estimate is still correct but the standard error becomes numerically unstable by orders of magnitude. The notebook demonstrates this directly and fixes it via a rank-revealing QR decomposition that identifies and drops the exact redundant columns before fitting — the same fix is built into the notebook's reusable `twfe()` function, which additionally asserts that the treatment coefficient's own variance is a valid positive number (distinct from a handful of thin countries' own fixed-effect parameters occasionally being poorly identified — traced to Angola's and Sierra Leone's fixed effects in the baseline/drop-switching variants, and to Malawi's and Senegal's in the drop-short-series variant, always unrelated to the treatment estimate itself).

**Headline findings**: none of the 18 (pooled) specifications reach conventional statistical significance (closest: `structural_change` at 3-year resolution, country level, dropping switching intervals, p ≈ 0.063), but **signs are completely stable** — `within` is positive and `structural_change` is negative in every single specification, regardless of interval width, aggregation level, or robustness variant. The direction matches the descriptive event-study's own finding (`descriptive_event_study_decomposition.csv` above): `structural_change` shrinks after treatment in Manufacturing and Services. **A consistency check this file used to satisfy no longer applies**: under the level-form decomposition these regressions used to run on, the country-level ("Total") coefficient equaled exactly 3x the matching sector-level coefficient in every row, since `Total`'s outcome was literally the sum of the three sectors' own values for the same interval. Under the growth-rate-normalized decomposition now in place (see `mcmillan_rodrik_decomposition_interval{3,5}[_by_sector].csv` above), that per-row data identity holds only in level form, not after each level normalizes by its own different `LP` — so this ratio no longer applies, by construction, not because of noise (documented and demonstrated directly in the notebook, §7).

## `twfe_sector_interacted_results.csv`

The pooled regressions above constrain `Reciprocal`'s coefficient to be the same across all three sectors. This file relaxes that: `Reciprocal` is interacted with sector dummies (`Reciprocal × 1[sector=Agriculture]`, etc.) inside the same `country x sector`/`sector x year` fixed-effect structure, giving one coefficient per sector instead of one pooled number. 15 rows: 5 specifications (`ln_productivity` annual, `within`/`structural_change` × 2 interval widths) × 3 sectors, baseline sample only (switching-interval and short-series robustness variants were not repeated for this version).

| Column | Meaning |
|---|---|
| `outcome`, `interval` | `ln_productivity`/`annual`, or `within`/`structural_change` × `interval3`/`interval5` |
| `broad_sector` | `Agriculture`, `Manufacturing`, or `Services` |
| `beta`, `se`, `p` | That sector's own `Reciprocal` coefficient, cluster-robust (by country) SE, and p-value |

**Headline finding: Manufacturing's `structural_change` effect is negative and individually significant at 5-year resolution (-0.221, p ≈ 0.036) — the only individually significant coefficient in this file.** Its 3-year counterpart is the same sign, weaker (-0.096, p ≈ 0.077). The joint test that the three sectors' `structural_change` effects differ is itself significant at 5-year resolution (F = 3.66, p ≈ 0.042) — genuine evidence, not just a suggestive pattern; none of the other four joint tests (annual `ln_productivity`, `within` at either interval width, 3-year `structural_change`) reach conventional significance (all p ≥ 0.13). None of the sector-specific `within` coefficients are individually significant at either interval width — the closest is Manufacturing at 5-year resolution (+0.181, p ≈ 0.067). Agriculture's `structural_change` effect is positive at both resolutions and closest to significance at 5-year (+0.064, p ≈ 0.083) — the opposite sign from Manufacturing's; Agriculture maps to a single GGDC sub-sector, so this is *not* a trivial zero — it reflects how agricultural employment's share of the *whole country's* workforce moves, which can shift even with one sub-sector inside the broad-sector grouping, since sub-sector shares are normalized by country-total employment (see the McMillan–Rodrik decomposition files above). Manufacturing's own annual `ln_productivity` effect stays marginal (+0.303, p ≈ 0.057) — unaffected by any of the interval-decomposition redesign, since it comes from the annual panel directly. This finding has moved twice already this session as the interval decomposition methodology itself was rebuilt — a reminder that these sector-specific point estimates are sensitive to how productivity/decomposition is computed at small sample sizes, not just to the treatment itself.

## `twfe_interval_productivity_results.csv`

The §6.3 Design A component regressions above use 3-year and 5-year intervals because differencing amplifies noise in flow variables — but the §6.4.B baseline (`ln_productivity`, the sector-level *level*, not a flow) was originally run only on the annual panel. This file asks whether that choice matters: it re-runs the same `reciprocal`/`reciprocal_interval` regression on `ln(productivity)` using each broad sector's own **standalone** productivity at the start of each 3-year and 5-year interval (`productivity_1`/`country_productivity_1` — point-in-time at the calendar-grid edge year, `Q_b/L_b`, *not* the `LP_b` "contribution" quantity the `within`/`structural_change` regressions above use — see the McMillan–Rodrik decomposition files above), using the same `twfe()` helper and fixed-effect structure as the annual case. 4 rows: 2 interval widths × 2 levels (sector/country), baseline sample only.

| Column | Meaning |
|---|---|
| `interval` | `interval3` or `interval5` |
| `level` | `sector` (Agriculture/Manufacturing/Services) or `country` (the `"Total"` pseudo-sector rows) |
| `beta`, `se`, `p` | The `reciprocal_interval` coefficient on `ln(productivity)`, cluster-robust (by country) SE, and p-value |
| `n`, `n_clusters` | Regression sample size and number of country clusters |

**Headline finding: interval smoothing does not clearly help here, unlike for the §6.3.A flow components.** The point estimate is positive at every resolution (0.064 to 0.157) and never reaches significance, matching the annual baseline's own null result (β = 0.149, SE = 0.121, p = 0.22 — see `twfe_baseline_results.csv` above). Precision is a wash rather than a clean improvement: 3-year sector-level is the one specification that's *more* precise than annual (SE 0.118 vs. 0.121), but 3-year country-level, 5-year sector-level, and 5-year country-level are all *less* precise, with 5-year country-level the worst of the five (SE 0.139). This is the expected pattern for a level variable, which does not carry the differencing-amplified noise that motivated interval-smoothing the flow variables in the first place — confirmed directly, not just assumed.
