# CEPII Gravity Dataset

Downloaded 2026-08-20 from CEPII (Centre d'Études Prospectives et d'Informations
Internationales): https://www.cepii.fr/CEPII/en/bdd_modele/presentation.asp?id=8 —
version `V202211` (the November 2022 release, the most recent publicly posted at the
time of download).

This is the source used to build the **trade- and GDP-weighted exposure variables**
described in `docs/developing-country-trade-productivity.md` §6.2 step 5, via
[`notebooks/02_clean_cepii_gravity.ipynb`](../../notebooks/02_clean_cepii_gravity.ipynb).

## What's here

| File | Contents |
|---|---|
| `Countries_V202211.csv` | ISO3/country-name crosswalk (14 KB). |
| `Label_*.csv` (9 files) | Small lookup tables decoding categorical variable codes (legal origin, GDP/population data source, WTO RTA type and coverage). None are larger than 100 bytes. |
| `Gravity_documentation.pdf` | CEPII's own 46-page variable dictionary and methodology note. |

## What was deliberately left out, and why

The dataset's main file, `Gravity_V202211.csv`, is **~1.25 GB uncompressed** (the
source zip is ~207 MB) — far past GitHub's 100 MB per-file limit, so it is **not**
committed here, the same reasoning already applied to the two large Baier–Bergstrand
Stata files (`data/baier-bergstrand/README.md`).

**To reproduce `notebooks/02_clean_cepii_gravity.ipynb` from scratch:**

1. Download `https://www.cepii.fr/DATA_DOWNLOAD/gravity/data/Gravity_csv_V202211.zip` (~207 MB, no login required).
2. Extract `Gravity_V202211.csv` from it.
3. Place it at `data/cepii-gravity/Gravity_V202211.csv` (this exact path — it's covered by `.gitignore` so it won't accidentally get committed).
4. Re-run the notebook.

## `Gravity_V202211.csv` structure (confirmed by direct inspection)

- **4,699,296 rows**, one per `(origin, destination, year)`, spanning **1948–2021**.
- It is a **"squared" panel**: every `country_id` pair appears in every year of the
  full range, whether or not both countries existed yet that year. Existence is a
  separate pair of flag columns, `country_exists_o`/`country_exists_d` (1/0) — not
  encoded by omitting rows the way Baier–Bergstrand's `"NoCty"` string does. This
  matters in practice: an ISO3 code that has been reused across a historical
  territorial change (e.g. Czechoslovakia's 1993 split into `CZE`/`SVK`) gets **more
  than one row per year** for the same `iso3_o`/`iso3_d` unless both existence flags
  are filtered on together — see the notebook §4 for the concrete diagnostic.
- Key columns used by this project's notebook: `iso3_o`/`iso3_d` (modern ISO3, the
  join key against this project's African/Northern lists), `gdp_o`/`gdp_d` and
  `gdp_ppp_o`/`gdp_ppp_d` (current-USD and PPP-adjusted GDP, World Bank/IMF/Barbieri
  sourced), and three bilateral trade-flow sources: `tradeflow_imf_o`/`_d` (IMF DOTS,
  coverage from 1948 — the source this project uses, for its deep historical reach),
  `tradeflow_comtrade_o`/`_d` (UN Comtrade, from ~1962), and `tradeflow_baci`
  (CEPII's own reconciled BACI flow, from 1996 only). The `_o`/`_d` suffix on the
  IMF/Comtrade fields distinguishes the flow *as reported by the origin* from the same
  flow *as reported by the destination* (mirror statistics, which routinely disagree)
  — the notebook averages the two. Also used, on the African-country side only, as
  regression-stage controls rather than for the exposure calculation itself:
  `gdpcap_o`/`_d`, `pop_o`/`_d` (WDI, backfilled with Maddison for population),
  `pop_pwt_o`/`_d`/`gdp_ppp_pwt_o`/`_d` (Penn World Table alternates), and the
  unilateral `wto_o`/`_d`/`gatt_o`/`_d` membership dummies.
- **Non-null coverage within this project's 1950–2017 window**, among the
  African↔Northern pairs actually used: `tradeflow_imf` ~62–64% per direction, `gdp`
  ~90%. This confirms IMF DOTS as the right primary trade source for a study that
  starts well before BACI's 1996 coverage window.
- Also bundled: WTO-sourced `rta_type`/`rta_coverage` variables (Customs Union / FTA /
  Economic Integration Agreement / Preferential Scope Agreement, per
  `Label_rta_type_V202211.csv`) and `eu_o`/`eu_d`/`gatt_o`/`wto_o` membership flags —
  not used by this project's notebook (Baier–Bergstrand's own 0–6 depth scale remains
  the source of the `reciprocal` treatment variable), but available for a future
  cross-check if needed.

## Citation

If you use this data, cite:

- Conte, M., Cotterlaz, P., & Mayer, T. (2022). *The CEPII Gravity Database*. CEPII Working Paper No 2022-05.
