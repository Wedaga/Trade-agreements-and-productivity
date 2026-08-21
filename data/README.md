# Raw data sources

| Location | Contents |
|---|---|
| `Global-Productivity-Sectoral-Database.dta` | GGDC-style sectoral productivity panel — see below. |
| `baier-bergstrand/` | Baier–Bergstrand Economic Integration Agreement database — see that folder's README. |
| `cepii-gravity/` | CEPII Gravity dataset — see that folder's README. |
| `processed/` | Cleaned/merged outputs of the `notebooks/` — see that folder's README. |

## `Global-Productivity-Sectoral-Database.dta`

A country–sector–year panel: **103 countries, 1950–2017, 40,040 rows.** Structure confirmed by direct inspection (not independently verified against an external source document, since none was provided with the file):

- **Columns**: `country` (name, not ISO3), `sector` (categorical), `year`, `Value_added_nominal`, `Value_added_real`, `Employment`, `Labor_productivity_real`, `Labor_productivity_PPP`.
- **`sector`** takes 10 values: `Total` plus 9 sub-sectors — `1.Agriculture`, `2.Mining`, `3.Manufacturing`, `4.Utilities`, `5.Construction`, `6.Trade services`, `7.Transport services`, `8.Finance amd business services` [sic — typo in the source data itself], `9.Other services`. This is one sub-sector short of the standard 10-sector GGDC breakdown (Government services and Community/social/personal services appear combined into `9.Other services` here rather than kept separate).
- **`Total` is confirmed to equal the sum of the 9 sub-sectors** for both value added and employment (checked to float32 precision, ~1e-7 relative error) — a genuine partition, not an independently-sourced aggregate.
- **`Labor_productivity_real` is confirmed to equal `Value_added_real / Employment` exactly.** `Labor_productivity_PPP` is a ratio only — the source does not carry a separate PPP-terms value-added column, so recovering a PPP value-added figure for any further aggregation requires backing it out as `Labor_productivity_PPP × Employment`.
- **Missingness**: essentially complete, except Namibia (1960–1964) and Botswana (1964–1967), which have a handful of country-years with at least one missing sub-sector.
- **Country coverage varies by country**: some start as early as 1960 (Ghana, Egypt, Nigeria, South Africa, Tanzania); others as late as 1999–2001 (Algeria, Sierra Leone).
- **Country names, not ISO3 codes** — joining against this project's other ISO3-keyed sources requires an explicit name→ISO3 crosswalk (see `notebooks/03_clean_ggdc_productivity.ipynb`). One name doesn't match the obvious pattern: Tanzania is listed as "United Republic of Tanzania".
- **Units are inherited as given, not independently re-verified** — employment is presumed to be in thousands of persons (standard GGDC convention) based on plausible magnitude checks against known population figures, not confirmed from source documentation.

See [`notebooks/03_clean_ggdc_productivity.ipynb`](../notebooks/03_clean_ggdc_productivity.ipynb) and [`processed/README.md`](processed/README.md#ggdc_africa_broad_sectorscsv) for how this gets aggregated to the three broad sectors (Agriculture/Manufacturing/Services) used in the McMillan–Rodrik decomposition.
