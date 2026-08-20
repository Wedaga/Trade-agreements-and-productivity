# Baier–Bergstrand Database on Economic Integration Agreements

Downloaded 2026-08-20 from the NSF–Kellogg Institute Database on Economic Integration Agreements maintained by Scott L. Baier and Jeffrey H. Bergstrand: https://sites.nd.edu/jeffrey-bergstrand/database-on-economic-integration-agreements/ (July 31, 2021 release — the most recent version publicly posted at the time of download).

## What's here

| File | Contents |
|---|---|
| `EIA_Database_2021-07-31.xlsm` | The panel dataset itself (23 MB). Sheets: `Cover`, `Definitions`, `Country List`, `Data Sheet` (the actual panel — see below), `Sheet1`, `Comments & PDF links`. |
| `Country_List_2021-07-31.xls` | Country name ↔ ID crosswalk used throughout the database. |
| `Overview_of_Database_2021-07-31.pdf` | The authors' own guide to the workbook's tabs and how to sort/filter it. |
| `Data_Construction_Methodology_2021-07-31.pdf` | How the 0–6 integration-depth index was constructed and coded. |
| `READ_ME_FIRST.pdf` | The authors' top-level orientation note. |

## `Data Sheet` structure (confirmed by direct inspection)

- **78 columns**: 10 identifier columns (`Exporter`, `IDEX`, `ISOEX`, `Importer`, `IDIM`, `ISOIM`, `IDPAIR (Relative)`, `IDPAIR`, `Countries`, `Sort Pair`) followed by one column per year, **1950 through 2017** (confirmed directly — the July 2021 release refreshed and re-validated the data but did not extend the year coverage past 2017).
- **37,830 populated rows** — country pairs listed in **both directions** (e.g., both "Belgium→France" and "France→Belgium" appear as separate rows; per the authors' own documentation, both-direction listing was added as of the May 31, 2013 release). The authors' overview document reports "23,201 pairings over 68 years" as the underlying pair count.
- Each year cell holds the integration depth as of that year, coded 0–6 (full definitions on the `Definitions` sheet, sourced to Frankel (1997), *Regional Trading Blocs in the World Economic System*):

  | Level | Code | Definition | Reciprocal? |
  |---|---|---|---|
  | 0 | — | No agreement | — |
  | 1 | NR-PTA | Non-reciprocal preferences from a developed country | **No** |
  | 2 | PTA | Reciprocal preferential arrangement | Yes |
  | 3 | FTA | Free trade area | Yes |
  | 4 | CU | Customs union | Yes |
  | 5 | CM | Common market | Yes |
  | 6 | EUN | Economic union | Yes |

  **Confirmed from the authors' `Data_Construction_Methodology_2021-07-31.pdf`: level 1 (non-reciprocal) specifically includes US GSP (from 1976), EU GSP (from 1976), and the African Growth and Opportunity Act — AGOA — (from 2000), among other unilateral preference programs.** For an African-focused project this is a first-order fact, not a footnote: for most Sub-Saharan African countries, the bulk of their recorded EU/US relationship across 1976–2016 sits at level 1, since reciprocal EU EPAs mostly entered into force only in the 2010s. A treatment variable built only from "FTA-or-deeper" (level ≥3) would exclude AGOA — and most of the GSP era — entirely. See `docs/developing-country-trade-productivity.md` §6.2 for how this project handles the distinction.
- Cells sometimes read `"NoCty"` instead of a number, meaning that country did not yet exist (or was not internationally recognized) in that year.
- The same methodology document reports that, after using bilateral trade-flow evidence to rule out agreements where no other documentation existed, the authors could not fully verify agreement status for roughly **1.4% of all country-pair/year cells**, which default to "no agreement." Worth a light robustness check on affected pairs rather than inheriting silently.

## What was deliberately left out, and why

The authors' official download is a single 1.6 GB zip archive. The **overwhelming majority of that size (≈1,900 files) is a `PDF Files` folder of the underlying treaty documents** that individual cells in the "with links" workbook hyperlink to — not part of the panel data itself. To keep this repository usable in plain git (GitHub hard-blocks any file over 100 MB), only the actual data/documentation files were extracted:

- The **`(without links)`** version of the workbook was kept rather than `(with links)` — without the treaty-PDF folder in the repo, those hyperlinks would just be broken; the underlying index values are identical between the two files.
- The two Stata-format copies (`..._Stata 15, 14 Version.dta` and `..._Stata 13 Version.dta`, ~226 MB each) were **not** included — they exceed GitHub's 100 MB per-file limit and are redundant with the `.xlsm` file kept here. If you need a `.dta` file, re-export it from the workbook, or re-download the underlying zip directly from the source above and convert.
- The ~1,900 individual treaty PDFs were **not** included. If you need the supporting documentation for a specific agreement, the source zip (linked above) has them, organized to match the `Comments & PDF links` sheet.

## Citation

If you use this data, cite the authors' underlying methodology papers (see `docs/developing-country-trade-productivity.md` in this repo for full citations):

- Baier, S. L., & Bergstrand, J. H. (2004). Economic Determinants of Free Trade Agreements. *Journal of International Economics*, 64(1), 29–63.
- Baier, S. L., & Bergstrand, J. H. (2007). Do Free Trade Agreements Actually Increase Members' International Trade? *Journal of International Economics*, 71(1), 72–95.
