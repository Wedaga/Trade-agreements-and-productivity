# North–South Trade Agreements and Sectoral Productivity in African Developing Countries
### Literature Review and Proposed Methodology

*Prepared: August 2026*
*Scope: this project studies how developing **African** countries' sectoral productivity responds to trade agreements with developed ("Northern") economies, and — as its core empirical strategy — decomposes that productivity response into within-sector and structural-change (labor-reallocation) components using the McMillan–Rodrik procedure. Agreements between two developing countries (e.g., COMESA, SADC, ECOWAS) are outside the core scope and are discussed only as contrast/context.*

---

## 1. Research Motivation

Three literatures bear on this project, and each is well developed on its own but has not, on the evidence of this review, been combined the way this project proposes:

- A **North–South PTA and productivity literature** exists, but its African evidence is thin and largely restricted to single-agreement case studies (AGOA) or trade-flow effects (EU–Africa EPAs) rather than productivity effects.
- A **structural-change / McMillan–Rodrik decomposition literature** has been applied to Africa specifically, more than once, using data from the same GGDC family this project holds — but always with trade treated, at most, as a broad "openness" control, never as a *trade-agreement* treatment variable with agreement-specific timing.
- A **general (non-African) literature** establishes the theoretical channels — reallocation, input access, learning-by-exporting — and the modern econometric methods needed to estimate PTA effects credibly despite staggered, non-random treatment timing.

The review below covers all three, flags with **★ Closest precedent** the papers nearest to this project's actual design, and closes with an explicit gap statement (§4) and a methodology (§6) built around confirming or refuting the project's stated plan: decompose sectoral productivity growth in African economies with the McMillan–Rodrik procedure, then test whether entering a trade agreement with a developed-world partner shifts the within-sector and/or structural-change components of that decomposition.

---

## 2. Literature Review

### 2.1 Measuring trade agreements: the Baier–Bergstrand database and related work

**Baier, S. L., & Bergstrand, J. H. (2004). "Economic Determinants of Free Trade Agreements." *Journal of International Economics*, 64(1), 29–63.**
Shows PTA formation is well predicted by low bilateral trade costs, larger combined economic size, and similarity in partner size, correctly classifying ~85% of existing FTAs and ~97% of non-agreement pairs. Establishes that **PTA formation is not random** — the covariates here (size, distance, similarity) are the natural controls for the endogeneity checks in §6.4.E.

**Baier, S. L., & Bergstrand, J. H. (2007). "Do Free Trade Agreements Actually Increase Members' International Trade?" *Journal of International Economics*, 71(1), 72–95.**
First long-run treatment-effect estimates of FTAs on bilateral trade using matching econometrics rather than pure cross-section gravity, showing naive cross-sectional models understate FTA effects because they ignore the endogeneity of agreement formation.

**Baier, S. L., Bergstrand, J. H., & Feng, M. (2014). "Economic Determinants of Free Trade Agreements Revisited: Distinguishing Sources of Interdependence." *Review of International Economics*, 22(1), 31–58.**
Separates *global* interdependence (multilateral trade-cost changes) from *bilateral* determinants of PTA formation — a possible source of instruments (third-country PTA exposure) for a given African country's own PTA formation.

**The NSF–Kellogg Institute Database on Economic Integration Agreements (Baier & Bergstrand).**
The trade-policy data source for this project: a **country-pair–year panel, 1950–2017, ~195 countries**, coding agreement depth on a 0–6 ordinal scale — 0 = none, 1 = non-reciprocal preferences (e.g., GSP, AGOA), 2 = reciprocal preferential arrangement, 3 = FTA, 4 = customs union, 5 = common market, 6 = economic union (full definitions and the reciprocal/non-reciprocal distinction in §6.2) — each entry linked to underlying treaty text. Because it is bilateral, it must be collapsed to a country-level "Northern PTA exposure" measure before merging with sectoral productivity data (§6.2).

**Mattoo, A., Rocha, N., & Ruta, M. (Eds.) (2020). *Handbook of Deep Trade Agreements*. World Bank.**
Codes which of ~50+ policy areas (services, investment, IP, technical barriers to trade, sanitary/phytosanitary measures, etc.) each agreement covers and whether provisions are legally enforceable — the natural complement to the 0–6 Baier–Bergstrand depth scale, letting the project distinguish a shallow tariff-only agreement (e.g., many older EU/US preference schemes for Africa) from a deep one (e.g., recent EU EPAs with services/investment chapters).

### 2.2 Theoretical channels linking trade agreements to productivity

**Melitz, M. J. (2003). "The Impact of Trade on Intra-Industry Reallocations and Aggregate Industry Productivity." *Econometrica*, 71(6), 1695–1725.**
Trade exposure reallocates resources from the least to the most productive firms within an industry, raising industry-average productivity even without any single firm becoming more efficient — the theoretical basis for expecting reallocation-driven, not just technology-driven, productivity effects, which is exactly what the McMillan–Rodrik decomposition (§2.6, §6.3) is built to detect at the sector level.

**McMillan, M., & Rodrik, D. (2011). "Globalization, Structural Change and Productivity Growth." NBER Working Paper 17143.** and **McMillan, M., Rodrik, D., & Verduzco-Gallo, Í. (2014). "Globalization, Structural Change, and Productivity Growth, with an Update on Africa." *World Development*, 63, 11–32.** ★ *Method precedent*
The source of the decomposition procedure the project plans to use: aggregate labor productivity growth is split into a *within-sector* component (sectors getting more efficient at what they already do) and a *structural-change* component (labor moving between sectors of different productivity levels). Applied to Africa, both papers find structural change has often been *growth-reducing* since 1990 — labor moving toward, not away from, low-productivity activities — concentrated in resource-exporting countries, with competitive exchange rates and labor-market flexibility associated with growth-enhancing reallocation instead. This supplies both the method (§6.3) and a live African hypothesis: a Northern PTA's productivity payoff may show up specifically by *reversing* this growth-reducing pattern — pulling labor toward tradable manufacturing/services rather than away from it.

**North–South trade, knowledge spillovers, and the "natural trading partners" hypothesis.**
Argues technology spillovers from trade are regional and partner-specific — a developing country gains most from trading with the advanced economy it is closest to institutionally or geographically. For African countries, the EU is the obvious "natural" Northern partner by this logic, which is the theoretical basis for expecting EU-linked agreements (EPAs, GSP+, prior Cotonou/Lomé arrangements) to show larger effects than, say, agreements with more distant Northern partners.

### 2.3 Firm- and plant-level evidence on trade liberalization and productivity (non-African, foundational)

**Topalova, P., & Khandelwal, A. (2011). "Trade Liberalization and Firm Productivity: The Case of India." *Review of Economics and Statistics*, 93(3), 995–1009.**
India's 1991 tariff reform raised firm productivity through both import competition (output tariffs) and cheaper/better intermediate inputs (input tariffs), the latter larger. Lesson for this project: **split PTA exposure by final-goods vs. intermediate-goods channels** where possible, rather than a single undifferentiated treatment.

**Amiti, M., & Konings, J. (2007). "Trade Liberalization, Intermediate Inputs, and Productivity: Evidence from Indonesia." *American Economic Review*, 97(5), 1611–1638.**
A 10-point cut in input tariffs raised productivity ~12% for input-importing firms — at least twice the output-tariff effect. Reinforces that the most plausible mechanism for *North–South* agreements specifically is African firms' access to Northern intermediate inputs and embodied technology, not just export market access.

**De Loecker, J. (2007). "Do Exports Generate Higher Productivity? Evidence from Slovenia." *Journal of International Economics*, 73(1), 69–98.** and **De Loecker, J. (2013). "Detecting Learning by Exporting." *American Economic Journal: Microeconomics*, 5(3), 1–21.**
Develops methods allowing productivity to evolve endogenously with export status, finding genuine learning-by-exporting effects. A caution against attributing observed productivity-PTA correlations to a causal channel without addressing reverse causality — reinforcing the event-study/staggered-DiD design in §6.3.

### 2.4 Trade agreements and productivity in developing countries generally

**Choudhri, E. U., & Hakura, D. S. "International Trade and Productivity Growth: Exploring the Sectoral Effects for Developing Countries." IMF Working Paper.**
Using a Krugman-style "technological gap" model, finds import competition raises productivity growth in *medium-growth* manufacturing sectors specifically, not low- or high-growth ones — direct precedent for expecting non-uniform, sector-specific effects rather than a single average treatment effect across all African manufacturing.

**Neri-Lainé, M., Orefice, G., & Ruta, M. (2023). "Deep Trade Agreements and Heterogeneous Firms' Exports." World Bank Policy Research Working Paper 10277.**
Using firm exports for 31 developing countries (2000–2020) matched to the Deep Trade Agreements database, finds moving from a shallow to a deep agreement raises exports ~3.6% on average, entirely driven by large, GVC-connected firms; small, less-connected firms are, on average, hurt. Evidence that **agreement depth matters and effects are heterogeneous by firm/sector connectivity** — a caution relevant to African economies, where GVC connectivity is low outside a handful of manufacturing hubs (e.g., Lesotho/Eswatini apparel, Ethiopian light manufacturing).

### 2.5 Trade agreements, exports, and productivity in Africa specifically

**Mulangu, F. M. (2012). "Preferential Trade Agreements, Employment, and Productivity: Evidence from AGOA." ACET Working Paper.** ★★ *Closest precedent*
Studies the U.S. African Growth and Opportunity Act — a unilateral-but-conditional Northern preference scheme, not a reciprocal PTA — and finds it raised Sub-Saharan African exports to the U.S. sharply and had a positive effect on firm productivity, partly via resources reallocating toward more productive firms, though employment gains were confined to very large firms. This is the closest existing paper on the *North–South + Africa + productivity* combination, but it (i) covers only one preference scheme rather than the full Baier–Bergstrand universe of African countries' Northern agreements, (ii) is firm-level rather than sector-level, and (iii) does not use the McMillan–Rodrik within/structural-change decomposition — it is the paper this project most directly extends.

**EU–SADC Economic Partnership Agreement trade-growth studies (e.g., an MDPI empirical assessment, 2022) and EU–ACP EPA empirical evidence for Sub-Saharan Africa.**
This literature quantifies *trade* effects of EU EPAs (trade creation vs. diversion, tariff-revenue losses, export growth by product), generally finding EPAs raise exports for a narrow set of highly disaggregated products (textiles, some agricultural goods) but impose real fiscal costs (customs-revenue losses reported as high as ~20% of government revenue for some West African economies) and mixed net welfare effects. Useful both as a source of specific North–South African agreements to code and as a caution that trade *effects* and *fiscal/productivity* effects can point in different directions — this literature stops at trade flows and does not extend to sectoral productivity.

**South-South regional agreements — ECOWAS, SADC, COMESA (trade-openness/growth studies, e.g., Heliyon 2021; trade-creation/diversion assessments of COMESA/SADC/ECCAS/ECOWAS).**
This is the *largest* body of empirical work on African regional trade agreements, but it is explicitly South–South (intra-African) rather than North–South, generally studies aggregate trade openness or GDP growth rather than sectoral productivity, and typically finds weak, sometimes statistically insignificant growth effects. Its main value to this project is as a **contrast group and scope boundary**: it confirms that African RTA research to date has concentrated on intra-African integration, leaving the North–South productivity question comparatively unexplored — reinforcing the gap identified in §4.

### 2.6 Structural change and productivity decomposition in Africa

**de Vries, G., Timmer, M., & de Vries, K. (2015). "Structural Transformation in Africa: Static Gains, Dynamic Losses." *Journal of Development Studies*, 51(6), 674–688.** ★★★ *Closest precedent (method)*
Introduces the **GGDC Africa Sector Database** (11 Sub-Saharan African countries, 1960–2010) and applies a McMillan–Rodrik-style decomposition, finding that resources reallocated into high-productivity-growth manufacturing in the early post-independence period, structural change then *stalled* from the mid-1970s, and when it resumed in the 1990s workers moved mainly into distributive trade services rather than manufacturing — a shift the authors characterize as offering static productivity gains (moving to a higher-productivity sector *today*) without dynamic gains (that sector is not one where productivity itself grows quickly over time), hence "static gains, dynamic losses." This is the closest methodological precedent in the entire review: same decomposition method, same country set (a subset overlaps directly with the 24 African countries in this project's own GGDC file, §5.1), same broad time span — but trade is not a treatment variable in this paper at all.

**Diao, X., Harttgen, K., & McMillan, M. (2017). "The Changing Structure of Africa's Economies." NBER Working Paper 23021 / World Bank Policy Research Working Paper 7958 / *The World Bank Economic Review*, 31(2), 412–433.** ★★★ *Closest precedent (method + scale)*
Uses the GGDC 10-Sector Database — the same database family as this project's own data — for **39 African countries** through ~2010–2012, applying the within/structural-change decomposition to compare Africa's structural-change patterns with the rest of the developing world. This paper's sample (39 African countries via GGDC 10-Sector data) is the closest existing match to what this project's own 24-country African subsample would look like extended to the full GGDC country list — again, without a trade-agreement treatment variable.

**Diao, X., McMillan, M., & Rodrik, D. (2017/2019). "The Recent Growth Boom in Developing Economies: A Structural-Change Perspective." NBER Working Paper 23132; also in the *Palgrave Handbook of Development Economics* (2019).**
Extends the decomposition across Latin America, Africa, and South Asia, finding recent African growth accelerations were driven by growth-*enhancing* structural change but — unlike East Asia's industrialization-led growth — this came at the expense of *declining* within-sector productivity growth in Africa's more modern sectors, with the forces promoting structural change originating on the demand side (external transfers, rising agricultural incomes) rather than from trade or industrial policy. This "African paradox" (reallocation and within-sector upgrading working in *opposite* directions) is a specific, testable pattern this project can check against the North–South-PTA treatment: does entering a Northern agreement break this paradox by supporting within-sector upgrading in modern tradable sectors, or does it leave the paradox intact?

**A country-level application of the McMillan–Rodrik decomposition to South Africa (Springer Nature, 2020 book chapter).**
Applies the decomposition to a single African economy, confirming the method's applicability at the individual-African-country level, again without a trade-agreement treatment variable. Useful as a template for single-country validation exercises before scaling the analysis to the full African panel.

### 2.7 Global value chains and North–South integration

**Pahl, S., Timmer, M., Gouma, R., & Woltjer, P. (2022). "Jobs and Productivity Growth in Global Value Chains: New Evidence for Twenty-Five Low- and Middle-Income Countries." *The World Bank Economic Review*, 36(3), 670–686.**
GGDC-affiliated work showing GVC-embedded jobs are systematically more productive than non-GVC jobs and that the *pace* of GVC expansion correlates with aggregate productivity growth, both across and within the (partly African) country sample. Shares data lineage with this project's own database.

**"Global value chains and aggregate productivity growth in developing countries: the role of intra-sectoral allocation and structural change." *Review of World Economics*, 161(1), 89–119 (2024).**
Panel fixed-effects and IV estimation on 46 developing countries decomposing GVC-driven productivity growth McMillan–Rodrik-style, finding effects present in Africa but strongest in Asia and Latin America — a further confirmation that the within/structural-change decomposition is the field's standard tool for exactly this kind of question, and a template IV strategy for trade-integration variables that could be adapted for PTA exposure.

### 2.8 Econometric methods for estimating trade-agreement effects

**Yotov, Y. V., Piermartini, R., Monteiro, J.-A., & Larch, M. (2016). *An Advanced Guide to Trade Policy Analysis: The Structural Gravity Model*. WTO/UNCTAD.**
Standard reference for PPML gravity estimation with exporter–time/importer–time/pair fixed effects — relevant to the intermediate step of constructing a trade-flow-based PTA exposure measure, and to good-practice fixed-effects/clustering conventions.

**Egger, P. H., Larch, M., & Yotov, Y. V. "Gravity-Model Estimation with Time-Interval Data: Revisiting the Impact of Free Trade Agreements."**
Shows 3–5-year-interval gravity estimates resemble annual ones, while naive pooled annual panels can produce biased trade-cost elasticities — informs the choice of annual vs. interval frequency for the productivity panel (§6.3).

**Sellner, R., & Yotov, Y. V., et al. (2023/24). "Staggered Difference-in-Differences in Gravity Settings: Revisiting the Effects of Trade Agreements." *American Economic Journal: Applied Economics*.** ★ *Method precedent*
Shows standard panel gravity (two-way fixed effects, TWFE) estimates of RTA effects are biased — in their sample, downward by more than 50% — because of staggered treatment timing and heterogeneous treatment effects, fixed with an extended two-way-fixed-effects (ETWFE) estimator. Because African countries in this project's sample enter Northern PTAs in different years, this is the direct justification for a staggered-DiD estimator as the primary specification (§6.4.C), not a plain TWFE regression.

**Callaway, B., & Sant'Anna, P. H. C. (2021). "Difference-in-Differences with Multiple Time Periods." *Journal of Econometrics*, 225(2), 200–230.**
The general econometric foundation behind the gravity-specific paper above: with staggered timing, TWFE is a weighted average of comparisons that can carry *negative* weights, so a uniformly positive true effect can appear negative or badly mis-sized. Callaway–Sant'Anna instead estimate group-time average treatment effects against not-yet-treated units, aggregated into an overall or dynamic effect — the recommended default estimator for the project's binary `Reciprocal_c,t` treatment (§6.2–6.3).

**Callaway, B., Goodman-Bacon, A., & Sant'Anna, P. H. C. "Difference-in-Differences with a Continuous Treatment." NBER Working Paper 32117.**
Extends the staggered-adoption DiD framework above to a continuous or ordinal dose rather than a binary treatment, estimating an average causal response as a function of dose under a generalized parallel-trends assumption that must hold at every dose level. Not needed for the project's primary specification, which uses a single binary treatment anchored at the reciprocal/non-reciprocal line specifically to avoid this complication (§6.2) — but available as an optional robustness check if the full 0–6 depth score is later explored as a dose-response extension of the §6.4.D heterogeneity analysis.

**de Chaisemartin, C., & D'Haultfœuille, X. (2020). "Two-Way Fixed Effects Estimators with Heterogeneous Treatment Effects." *American Economic Review*, 110(9), 2964–2996; extended in follow-up work to non-binary and non-staggered (switching) treatments.**
Shows TWFE regressions estimate a weighted sum of group-time treatment effects with potentially *negative* weights (so a regression coefficient can be negative even when every unit's true effect is positive), and proposes an alternative that generalizes the event-study approach by defining the event as the period a unit's treatment first *changes*, in either direction. Relevant background for why preference-scheme eligibility (e.g., AGOA, genuinely suspended and reinstated for real countries in the sample) is not treated as the primary variable in this project (§6.2); available as an optional estimator if the non-reciprocal-only heterogeneity cut in §6.4.D is later analyzed with its own dynamic specification rather than simple subsample comparison.

---

## 3. Papers closest to this project

Ranked by closeness to the actual combination this project proposes (North–South PTA treatment × African sectoral productivity × McMillan–Rodrik decomposition):

1. **de Vries, Timmer & de Vries (2015), "Structural Transformation in Africa: Static Gains, Dynamic Losses."** Same method, same African country set (overlapping with this project's own 24-country African subsample), same GGDC data lineage. Closest on *method and geography*; has no trade-agreement treatment variable at all.
2. **Diao, Harttgen & McMillan (2017), "The Changing Structure of Africa's Economies."** Same decomposition method, same database family (GGDC 10-Sector), larger African sample (39 countries) closely matching this project's own coverage. Closest on *method and scale*; again, no trade-agreement variable.
3. **Mulangu (2012, ACET), "Preferential Trade Agreements, Employment, and Productivity: Evidence from AGOA."** The only paper found here combining Africa, a North–South preference arrangement, and a productivity outcome. Closest on *substantive question*; but single-agreement, firm-level, and does not use the McMillan–Rodrik decomposition.
4. **Diao, McMillan & Rodrik (2017/2019), "The Recent Growth Boom in Developing Economies."** Documents the specific African pattern (growth-enhancing reallocation paired with declining within-sector productivity in modern sectors) that a trade-agreement treatment could plausibly interact with or reverse. Closest on *generating a testable African-specific hypothesis* for the decomposition.
5. **Neri-Lainé, Orefice & Ruta (2023), "Deep Trade Agreements and Heterogeneous Firms' Exports."** Not African-specific and not a productivity study, but closest on the *treatment side*: it operationalizes agreement depth as a firm/export outcome predictor and finds effects concentrated among GVC-connected firms — a heterogeneity result this project should expect to see echoed in which African sectors respond to PTA depth.

No paper identified in this review sits at the intersection of all three elements simultaneously — that intersection is the gap this project addresses (§4).

---

## 4. Is there a significant gap in the literature? A direct assessment

**Yes — based on the literature surveyed here, there is a clear and specific gap, though it is a gap of *combination* rather than of any single missing ingredient.** Each component of this project's design exists on its own:

- Africa-specific McMillan–Rodrik-style decompositions exist and use the same GGDC data family this project holds (de Vries, Timmer & de Vries 2015; Diao, Harttgen & McMillan 2017; Diao, McMillan & Rodrik 2017) — but in every case reviewed, trade enters the analysis, if at all, as a broad "openness" control (e.g., trade as a share of GDP), never as a **trade-agreement treatment variable with its own entry-into-force timing**, and never distinguishing North–South from South–South agreements.
- A North–South PTA/productivity literature exists and covers Africa in at least one case (Mulangu 2012 on AGOA) — but it is a single-agreement, firm-level case study, not a systematic panel covering the full set of African countries' agreements with the developed world (which the Baier–Bergstrand database, uniquely, makes possible), and it does not decompose productivity into within-sector vs. structural-change components.
- The largest body of *African regional trade agreement* research (COMESA, SADC, ECOWAS trade-creation and growth studies) is explicitly South–South and stops at trade flows or aggregate GDP growth, not sectoral productivity, let alone its decomposition.
- The methodological literature needed to estimate this credibly (staggered-DiD-robust estimators for non-randomly-timed agreement adoption) is mature and directly transferable (Sellner & Yotov; Callaway & Sant'Anna), but has not, on this evidence, been applied to an African sectoral-productivity outcome.

Put together: **no paper found in this review measures whether developing African countries' entry into agreements with developed-world partners shifts the balance between within-sector productivity upgrading and structural (labor-reallocation) change, at the sector level, across the continent, using a systematic and depth-graded measure of agreement treatment.** That is a real and defensible gap, and it is precisely the question this project's data (GGDC sectoral productivity for ~24 African countries + Baier–Bergstrand's full bilateral PTA panel) and stated method (McMillan–Rodrik decomposition applied around PTA treatment) are positioned to fill. Two caveats are worth stating plainly: this is a literature *review*, not an exhaustive systematic search, so a working paper doing exactly this could exist unpublished or outside the sources searched here; and the gap is a gap in *combination*, so the project's contribution should be framed and defended as integrating and extending these adjacent literatures, not as introducing an entirely new method or dataset.

---

## 5. The data already in hand

### 5.1 `Global-Productivity-Sectoral-Database.dta`

Confirmed by direct inspection to be (or be built from) the **GGDC 10-Sector Database**: 103 countries, 1950–2017, 9 sectors plus a "Total" (Agriculture; Mining; Manufacturing; Utilities; Construction; Trade services; Transport services; Finance and business services; Other services), with `Value_added_nominal`, `Value_added_real`, `Employment`, `Labor_productivity_real`, and `Labor_productivity_PPP` per country–sector–year cell.

**24 of the 103 countries are African**, giving this project a ready-made African panel without needing a new data pull: Algeria, Angola, Botswana, Burkina Faso, Cameroon, Egypt, Eswatini, Ethiopia, Ghana, Kenya, Lesotho, Malawi, Mauritius, Morocco, Mozambique, Namibia, Nigeria, Rwanda, Senegal, Sierra Leone, South Africa, Tanzania, Uganda, and Zambia. This spans Sub-Saharan and North African economies, oil/resource exporters (Algeria, Angola, Nigeria) and diversified manufacturers (South Africa, Mauritius, Kenya), giving real cross-sectional variation for the heterogeneity analysis in §6.3.D.

### 5.2 The year-limit problem, and alternative/extension datasets

Both core sources nominally stop around 2017, which cuts the analysis window off just before AfCFTA (South–South, so outside core scope regardless), most CPTPP/USMCA-era agreements, and a few years of GGDC/EU-EPA-related PTA activity. Rather than treat this as fixed, three actively maintained alternatives are worth evaluating before finalizing the sample window — one on the productivity side, two on the trade-agreement side.

**Sectoral productivity — the GGDC/UNU-WIDER Economic Transformation Database (ETD) is the strongest single alternative.** It is the direct successor to the GGDC 10-Sector Database this project already holds (produced by the same Groningen team, now jointly with UNU-WIDER), covers **51 countries — including 18 Sub-Saharan African and 4 North African/Middle Eastern countries, i.e., a comparable or larger African set than the current 24-country subsample — for 1990–2018, at 12 sectors instead of 9**, and was last updated in September 2023 (so it is being actively maintained, not a frozen legacy product). The extra year (2018 vs. 2017) is modest, but the finer sector split, broader and more current country list, and explicit design for structural-change research make it worth reconciling against the current file rather than treating as a separate afterthought — some countries or years may be available in one but not the other. Two further options, useful mainly as robustness checks rather than primary extensions: the **Expanded Africa Sector Database (EASD)** (Mensah & Szirmai, 2018) covers 18 African economies (≈80% of Sub-Saharan African GDP) from the 1960s to 2015, adding seven countries (Burkina Faso, Cameroon, Lesotho, Mozambique, Namibia, Rwanda, Uganda) beyond the original Africa Sector Database — still short of the 2017/2018 endpoint, but a useful cross-check on employment/value-added series; and **UNIDO INDSTAT2**, manufacturing-only but running to 2021, for extending the manufacturing sector specifically past 2018 if the analysis needs to reach the most recent years.

**Trade agreements — two alternatives can extend or cross-validate Baier–Bergstrand.** The **WTO's own Regional Trade Agreements Database (RTA-IS)** is the authoritative, continuously updated record of every agreement notified to the WTO, including exact entry-into-force dates (e.g., it records AfCFTA's legal entry into force as 30 May 2019, ahead of the January 2021 start of trading) — it lacks Baier–Bergstrand's/DESTA's depth coding but is the right source for confirming or extending PTA *timing* past 2017. The **DESTA (Design of Trade Agreements) database** (Dür, Baccini & Elsig) is a strong alternative or complement for *depth*: it covers 846 PTAs signed since 1945 with detailed content coding for 646 of them across 100+ design items, and its most recent public version is dated September 2025 — appreciably more current metadata than Baier–Bergstrand's, though the fine-grained design-feature coding itself is reported (per its own codebook) to run through roughly 2019, which should be confirmed directly before relying on it for later years. The Mattoo–Rocha–Ruta *Content of Deep Trade Agreements* database (§2.1) is also periodically updated beyond its original 2020 Handbook vintage and should be checked for its current end year.

### 5.3 Complementary databases for controls and mechanisms

| Purpose | Candidate source |
|---|---|
| Bilateral trade flows for exposure weighting | CEPII Gravity dataset (bundles IMF DOTS, UN Comtrade, and BACI) — **done**, §6.2 step 5 |
| GVC participation | ADB Multi-Regional Input-Output tables or UNCTAD-Eora26 (better African coverage than OECD TiVA) |
| Specific EU–Africa agreement variables (EPAs, GSP+, prior Cotonou/Lomé arrangements) | European Commission DG Trade agreement texts; the EU–SADC/EU–ACP EPA empirical literature (§2.5) as a coding cross-check |
| Standard controls | World Bank WDI, Barro–Lee schooling, Worldwide Governance Indicators |

### 5.4 A data-alignment issue to resolve early

Given §5.2, the practical recommendation is to **rebuild the core panel on the ETD (1990–2018, richer African coverage) rather than the original GGDC 10-Sector file**, cross-check PTA timing against the WTO RTA-IS database to catch anything the Baier–Bergstrand vintage misses between 2017 and 2018, and treat DESTA as the fallback for depth-coding if the Mattoo–Rocha–Ruta database's own end year turns out to be no later. Even with these upgrades, the window still stops around 2018–2019 and so still cannot capture AfCFTA — which, again, is South–South and outside this project's core North–South scope regardless.

**Update:** the Baier–Bergstrand file itself has now been downloaded into this repo (`data/baier-bergstrand/`, see that folder's README for full provenance) and inspected directly. Its "July 31, 2021" release date refers to when the data was last refreshed/re-validated, **not** an extension of coverage — the panel still runs 1950–2017 exactly as originally documented, confirmed by reading the actual column headers (68 year columns, 1950 through 2017, across 37,830 country-pair-direction rows). This closes the open question from the previous draft and confirms the ETD/WTO-RTA-IS/DESTA supplementation discussed above is necessary, not optional, if the analysis needs to reach past 2017.

---

## 6. Proposed methodology

### 6.1 Conceptual framework

Following §2.2–2.3 and §2.6, a Northern PTA can move African sectoral productivity through: (i) **market access** (export demand from the Northern partner), (ii) **input access** (cheaper/better intermediate goods and capital equipment — the strongest single channel per Amiti–Konings), (iii) **competition/reallocation** (import competition reallocating resources toward more productive domestic producers, per Melitz and Topalova–Khandelwal), and (iv) **regulatory/deep-agreement channels** specific to services, investment, and standards provisions (per Neri-Lainé, Orefice & Ruta). Crucially, per Diao, McMillan & Rodrik's "African paradox," reallocation and within-sector upgrading do not necessarily move together in Africa — a PTA could plausibly support one while leaving the other unchanged, which is exactly what the decomposition in §6.3 is designed to detect.

**Channels (ii) and (iii) require reciprocity.** Input access and import-competition/reallocation only operate if the African country itself lowers its own barriers — which, as §6.2 details, is not true of every arrangement in the data. This is not a minor caveat; it is the reason the treatment variable cannot simply be "any agreement" and is addressed directly below.

### 6.2 Sample and variable construction, and how the data's agreement *types* are handled

1. **Restrict the country sample to the 24 African countries identified in §5.1** (extending to the larger Diao–Harttgen–McMillan-style 39-country African panel or the GGDC Africa Sector Database if broader coverage is obtained).
2. **Classify each African country's trading partners into "Northern" (developed) vs. other**, using a fixed-vintage World Bank/IMF advanced-economies list.

**A terminology note.** This document uses "PTA" throughout as the generic umbrella term for any discriminatory/preferential trade arrangement — standard usage in the broader trade literature. This is *broader* than Baier–Bergstrand's own codebook, where "PTA" denotes specifically **level 2** on their 0–6 ladder (see below), one rung below a free trade agreement. When precision matters, this document refers to the specific level by name (e.g., "level-1 non-reciprocal preferences," "FTA-or-deeper").

**The six levels are not interchangeable for this project, and the split matters most at exactly the reciprocal/non-reciprocal line.** Confirmed directly from the authors' own construction methodology (now in `data/baier-bergstrand/`):

| Level | Code | Definition | Reciprocal (African country also liberalizes)? |
|---|---|---|---|
| 0 | — | No agreement | — |
| 1 | NR-PTA | "Preferential terms and customs concessions given by developed nations to developing countries" | **No.** Confirmed to include US GSP (from 1976), EU GSP (from 1976), and **AGOA (from 2000)** specifically. |
| 2 | PTA | "Preferential terms to members vs. non-members" | Yes |
| 3 | FTA | Free trade area — barriers eliminated among members | Yes |
| 4 | CU | Customs union | Yes |
| 5 | CM | Common market | Yes |
| 6 | EUN | Economic union | Yes |

The practical consequence for an African-focused project is large: **for most Sub-Saharan African countries, the bulk of their recorded relationship with the EU and US across 1976–2016 sits at level 1** (GSP, then AGOA from 2000), not level 2 or above — reciprocal EU Economic Partnership Agreements with African partners mostly entered into force only in the 2010s, near the end of the sample window. Two consequences follow, and together they fix *where* the primary treatment variable should be anchored:

- **AGOA — the subject of the closest precedent paper in this review (Mulangu 2012, §2.5) — is itself a level-1, non-reciprocal arrangement**, and non-reciprocal status has genuinely been reversed for real countries (Madagascar's AGOA eligibility was suspended in December 2009 after a coup and reinstated in 2014). A treatment variable anchored at level ≥3 (FTA-or-deeper) would exclude AGOA entirely; one anchored at level ≥1 (any relationship at all) would inherit this reversal as a real non-absorbing-treatment problem.
- Because GSP eligibility was extended to most African countries in the same short window around 1976, **an "any agreement" (level ≥1) treatment is also badly bunched in time** — nearly the whole sample would flip from 0 to 1 within a few years, leaving little genuine staggered variation for a Callaway–Sant'Anna-style estimator to exploit after the late 1970s, and very few not-yet-treated countries to serve as controls from that point on.

**Primary treatment: a single binary, anchored at the reciprocal/non-reciprocal line, not the ordinal score or the FTA line.** Construct

```
Reciprocal_c,t = 1 if the maximum level with any Northern partner is ≥ 2 (reciprocal PTA or above), else 0
```

3. This single variable is what carries the headline empirical strategy (§6.3–6.4) and the single staggered-DiD estimator discussed below. It is preferred over both alternatives considered above for the same two reasons at once: it is where the input-access and competition/reallocation channels (§6.1) actually switch on, *and* it happens to be close to absorbing (reciprocal commitments are essentially never revoked, so AGOA-style reversals — which move a country between 0 and 1 — never cross this particular boundary) with genuinely staggered timing (reciprocal EU EPA entry is spread across the 2000s–2010s rather than bunched). Treat the `NR-PTA+`/GSP+ sub-code (1.1 in the raw data) as level 1 (non-reciprocal) for this purpose.
4. **The ordinal 0–6 depth score, the FTA-or-deeper cut, and the individual agreement types are not separate primary specifications — they are reserved for the heterogeneity analysis in §6.4.D**, run as subsamples/interactions on top of the single `Reciprocal_c,t` design rather than as parallel treatment definitions each needing their own estimator. This is also where the AGOA/Mulangu-specific question (does the market-access-only, non-reciprocal case behave differently from full reciprocal entry?) gets asked — as a secondary cut, not the headline result.
5. **Trade- or GDP-weighted exposure** as a further robustness variant of the same binary — not required for the primary `Reciprocal_c,t` design, but useful for two things the binary/ordinal treatment cannot distinguish: (i) an FTA with an economically massive partner (e.g., the EU) versus one with a negligible partner (e.g., Malta alone), which look identical under a simple max-across-partners rule; and (ii) the cumulative breadth of a country's Northern relationships, which a naive partner *count* badly overstates for a structural reason confirmed directly in the data — see below. **Done** — built in `notebooks/02_clean_cepii_gravity.ipynb` from the CEPII Gravity dataset (`data/cepii-gravity/`), using IMF DOTS as the bilateral trade source (chosen over BACI/Comtrade for its deeper historical coverage back to 1948, well before BACI's 1996 start) alongside GDP. Two continuous `[0, 1]` variables result — `reciprocal_trade_share` and `reciprocal_gdp_share`, the share of a country's Northern trade/GDP exposure sitting with a reciprocal-level partner — saved to `data/processed/trade_agreements_with_exposure_country_year.csv`.

   **A prerequisite before building this, confirmed directly from the cleaned data (§6.6 sequencing, notebook `01_clean_trade_agreements.ipynb`): 23 of the 36 Northern partners are EU member states, and Baier–Bergstrand codes the EU's single common trade policy as 23 separate identical bilateral rows, one per member state, not one "EU" relationship.** Concretely, South Africa's FTA-level status and Ghana's PTA-level status each come entirely from 23 identical EU-member rows, not from 23 independent agreements. `Reciprocal_c,t` and `depth_score` are unaffected (a maximum over 23 identical values equals a maximum over one), but any measure that *counts* partners — including `n_northern_with_agreement` in the processed country-year file — is structurally inflated by EU membership count, and would need the EU's members collapsed to a single partner before being used as a genuine "breadth of Northern integration" measure. Going straight to bilateral trade-value weighting sidesteps this rather than needing a separate fix for it.
6. **Merge onto the African country–sector–year panel** by country and year. **Done** — `notebooks/04_build_estimation_panel.ipynb` merges the sector-level interval decomposition (§6.3) onto this panel, with trade-agreement controls attached at the *start* of each interval (avoiding "bad control" bias) and `reciprocal` at both interval endpoints to classify each interval as `never_reciprocal`/`always_reciprocal`/`switched_during_interval` — confirming directly that a reversion (the fourth logical case) never occurs, i.e. that `Reciprocal_c,t`'s absorbing property holds at interval resolution, not just annually. Validated against South Africa's 2000 EU TDCA entry: the 1999–2002 interval containing that date is correctly classified `switched_during_interval`, flanked by `never_reciprocal` (1996–1999) and `always_reciprocal` (2002–2005). Outputs at `data/processed/estimation_panel_interval{3,5}.csv` (documented in `data/processed/README.md`) — this is the dataset §6.4's regressions are meant to run on.
7. **Outcome variables**: log `Labor_productivity_real` (within-country growth specifications) and log `Labor_productivity_PPP` (cross-country level/convergence specifications); `Value_added_real` and `Employment` shares by sector retained separately as the direct inputs to the decomposition in §6.3.

One further data-quality note from the same methodology document: the authors could not fully verify agreement status for about 1.4% of all country-pair/year cells (after using trade-flow evidence to rule out agreements for the rest), and treat those as "no agreement" by default assumption — worth a light robustness check (e.g., dropping or flagging affected country-pairs) rather than an assumption to inherit silently.

**Does this still support the staggered-DiD estimator proposed in §2.8? Yes, cleanly, and as a single tool rather than several.** Because `Reciprocal_c,t` is anchored where reciprocal commitments essentially never revert and where timing is genuinely spread across years, standard Callaway–Sant'Anna (or Sellner & Yotov's gravity-adapted ETWFE) applies directly to the primary specification — no need for a continuous-treatment or switching-robust estimator as a *required* part of the core design. Before finalizing, confirm this empirically rather than by assumption (tabulate whether `Reciprocal_c,t` ever reverts for any country in the sample; it should be rare-to-none). Two extensions remain available as optional robustness checks, not core requirements, if the ordinal or non-reciprocal-only cuts in §6.4.D later turn out to be of independent interest: Callaway, Goodman-Bacon & Sant'Anna's continuous-treatment extension (for a dose-response version of the ordinal score) and de Chaisemartin & D'Haultfœuille's switching-robust estimator (for the level-1-only spells specifically, where AGOA-style reversals do occur).

### 6.3 The McMillan–Rodrik decomposition, and how trade agreements enter it

This is the confirmed core method. For each country and interval (t−k to t), aggregate labor productivity growth decomposes as (McMillan & Rodrik 2011, equation 1 — confirmed directly from the paper, p. 13, rather than assumed):

```
ΔP_t = Σ_i θ_i,(t−k) · Δp_i,t     +     Σ_i p_i,t · Δθ_i,t
        \_____________________/          \_____________/
           within-sector term               structural-change
                                             (reallocation) term
```

where `P` is economy-wide labor productivity, `θ_i` is sector *i*'s employment share, and `p_i` is sector *i*'s labor productivity (both directly available in the GGDC data, §5.1). **Correction**: an earlier version of this document had the structural-change term as `Δθ_i,t · p_i,(t−k)` (beginning-of-period productivity) — the paper's own equation uses `p_i,t`, the **end**-of-period level. This isn't a stylistic choice: pairing beginning-period employment shares with end-of-period productivity levels is exactly what makes the two terms sum to the *actual* change in economy-wide productivity with zero residual, confirmed numerically in `notebooks/03_clean_ggdc_productivity.ipynb` §10 (residuals of order 1e-12 to 1e-13, i.e. floating-point exact, not an approximation). Following Diao, McMillan & Rodrik (2017), a three-term dynamic version that also captures the covariance between sectoral productivity growth and employment-share growth can be used as a robustness check against this two-term "static" version (also used by de Vries, Timmer & de Vries 2015) — not yet implemented.

**Done**: `notebooks/03_clean_ggdc_productivity.ipynb` §10–§14 runs this decomposition on the African panel (both year-over-year and over each country's full observed window), as the replication sanity check called for in §6.6 step 3 — **at both the country level and, since this project's own research question is specifically about sectoral productivity, broken out by broad sector** (each sector's own within/structural-change contribution kept separate rather than collapsed into the country total). Country-level: qualitatively consistent with the literature, within-sector positive and dominant for most countries, structural change negative for 4 of 24 (Zambia, Eswatini, Sierra Leone, Angola), consistent with the "African paradox" pattern though not universal. Sector-level: a clean textbook pattern shows up (e.g. Ghana 1960–2017) of labor exiting agriculture (negative structural-change contribution) into manufacturing and especially services (positive contributions in both). Outputs at `data/processed/mcmillan_rodrik_decomposition_{annual,fullperiod,interval3,interval5}.csv` and the `..._by_sector.csv` counterparts (documented in `data/processed/README.md`). Not yet run against `Reciprocal_c,t` — that's designs (A)/(B) below, which require the still-pending merge with the trade-agreement panel (§6.2 step 6).

**Sector-level vs. country-level as the primary specification going forward**: sector-level should lead. The country-level totals were the right first step — a single, simple summary for the replication sanity check in §6.6 step 3, and they remain a useful robustness/headline layer. But this project's own research question, stated from the outset, is about *sectoral* productivity specifically, and the theoretical channels reviewed in §2.2–§2.3 (Melitz-style reallocation, input-access/competition effects, the enclave-sector hypothesis) all make sector-specific predictions — e.g. that import-competition effects concentrate in tradable manufacturing rather than spreading uniformly. A country-level total cannot distinguish "the agreement raised manufacturing productivity" from "the agreement raised agricultural productivity by the same amount," and the baseline sector-level regression already specified in §6.4.B/C (`ln(LaborProductivity_cst)`, a country–**sector**–year panel) was already built around this same choice before the decomposition work existed — so leading with sector-level here is consistent with, not a departure from, the design already in place. Concretely: run designs (A)/(B) with the `_by_sector` decomposition files' `within`/`structural_change` columns as the outcome, interacted with or split by `broad_sector`, as the primary specification; keep the country-level totals as a companion summary table.

**Annual vs. full-period vs. interval as the time resolution for estimation**: neither of the first two extremes is right. Annual differences are too noisy — both components are first differences of already-noisy series, and structural change specifically is a slow-moving process that's mostly noise at one-year resolution; full-period collapses to one row per country, leaving no panel structure for a staggered-adoption estimator to exploit at all. The right resolution is the **3-year and 5-year interval panels** already built (`..._interval3.csv`, `..._interval5.csv`, and their `_by_sector` counterparts) — a shared calendar grid of fixed-width bins applied identically to every country, matching this section's own "short (3–5 year)" specification and the Egger–Larch–Yotov convention it cites. Both widths are kept rather than picking one: 5-year smooths more noise; 3-year gives more periods to countries with short coverage. Checked directly: at 3-year resolution every one of the 24 countries has at least 5 usable bins; at 5-year resolution only 2 of 24 (Angola, Sierra Leone) fall below 3 bins. Given how few are affected — and that both are resource-dependent economies directly relevant to the §6.4.D resource-dependence heterogeneity cut — the recommendation is to keep them in the baseline sample (a staggered panel estimator pools identifying variation across units via its fixed effects, so a thin country loses precision, not identification) and treat their exclusion as a robustness check rather than a default drop.

**Applying the trade-agreement variable to the decomposition — two complementary designs:**

- **(A) Component-level panel regressions.** Compute `Δp_i,t` (within) and the reallocation term country-by-country over short (e.g., 3–5 year, per Egger–Larch–Yotov) intervals, then run the *same* staggered-DiD specification twice — once with the within-sector component as the outcome, once with the structural-change component — against `Reciprocal_c,t`. This directly tests whether a Northern PTA operates by raising within-sector productivity growth, by pulling labor toward higher-productivity tradable sectors, by both, or by neither (i.e., whether it breaks or reinforces the Diao–McMillan–Rodrik "African paradox").
- **(B) Pre/post event windows around each country's PTA entry.** For each African country, compute the full decomposition separately for the period before and after its first Northern-PTA entry-into-force date, and compare the within-sector and structural-change shares of total productivity growth across the two periods, benchmarked against not-yet-treated African countries in the same calendar years (avoiding the "never-treated" comparison-group problem flagged in §2.8).

**Estimating equation for the component regressions**, paralleling the baseline in §6.4 but with the decomposition components as outcomes:

```
Within_c,t     = β_W · Reciprocal_c,t + γ_c + δ_t + ε_c,t
Structural_c,t = β_S · Reciprocal_c,t + γ_c + δ_t + ε_c,t
```
with country and year fixed effects, estimated first by TWFE as a baseline and then by the staggered-adoption-robust estimator (Callaway–Sant'Anna or the ETWFE approach of Sellner & Yotov) as the primary specification, given the staggered timing of African countries' entry into reciprocal agreements. As a secondary heterogeneity cut (§6.4.D), re-run both equations comparing non-reciprocal-only spells against the reciprocal-treated group — if β_W is materially larger for the reciprocal case, that is direct evidence the input-access/competition channels (not just market access) are driving within-sector upgrading.

### 6.4 Empirical strategy for the underlying sector-level productivity regressions

**A. Descriptive event-study plots** of sectoral productivity around each country's first Northern-PTA entry, before any regression — the fastest check on pre-trends.

**B. Baseline two-way fixed-effects panel regression**, run as a comparison baseline only, given §2.8's bias warning:
```
ln(LaborProductivity_cst) = β · Reciprocal_ct + γ_cs + δ_st + ε_cst
```
country–sector fixed effects (γ_cs), sector–year fixed effects (δ_st), standard errors clustered by country.

**C. Staggered-adoption-robust estimator (primary specification)**, using not-yet-treated African countries as the comparison group, aggregated into an overall ATT and a dynamic event-study profile.

**D. Heterogeneity analysis**, all run as subsamples/interactions on top of the single `Reciprocal_ct` design in B/C rather than as separate treatment definitions:
- **By agreement type** — the piece deferred from §6.2: non-reciprocal-only spells (AGOA/GSP, directly extending Mulangu's finding) vs. reciprocal-treated spells, and, within the reciprocal-treated group, FTA-or-deeper (level ≥3) vs. reciprocal-PTA-only (level 2), paralleling Neri-Lainé, Orefice & Ruta's depth results (§2.4). Start with simple subsample re-estimation (same estimator, split sample); a richer version — defining Callaway–Sant'Anna "groups" jointly by type *and* first-treatment date rather than date alone — is a later extension, not a starting requirement.
- **By sector tradability** (manufacturing/tradable services vs. agriculture/mining vs. non-tradable services) and by content-based agreement depth (Mattoo–Rocha–Ruta score, §2.1), where merged.
- **By resource dependence** — given the African sample's split between resource exporters and diversified manufacturers (§5.1) — directly testing the McMillan–Rodrik "enclave sector" hypothesis against the treatment.

**E. Endogeneity checks**: control for the Baier–Bergstrand gravity determinants (size, similarity, distance); test for pre-trends via leads of treatment; consider a synthetic-control exercise for a small number of prominent single-country cases (as exists for AGOA); consider third-country PTA network exposure as an instrument (per Baier, Bergstrand & Feng 2014).

### 6.5 Suggested control variables

GDP per capita and growth; human capital (years of schooling); FDI inflows; institutional quality (WGI); real effective exchange rate; natural-resource export share (flagged by McMillan–Rodrik as suppressing growth-enhancing structural change); initial (pre-treatment) sectoral productivity level; and, as an extension, a GVC-participation measure to test whether Northern PTAs work partly *through* deepening GVC integration (§2.7). Also worth controlling for: the African country's own WTO/GATT accession status, since its timing could otherwise be confounded with `Reciprocal_c,t`.

**Partially done**: GDP per capita, population, and WTO/GATT membership are now built directly into `data/processed/trade_agreements_with_exposure_country_year.csv` (`notebooks/02_clean_cepii_gravity.ipynb` §7), sourced from CEPII Gravity's own WDI/Barbieri/Maddison/PWT-based fields. Still to be merged in from external sources: human capital, FDI, WGI institutional quality, real effective exchange rate, natural-resource export share, and GVC participation.

### 6.6 Practical sequencing

1. Confirm the actual year range of the downloaded Baier–Bergstrand file (done — confirmed 1950–2017, §5.4); extract the 24-country African subsample identified in §5.1 and check overlap/coverage against de Vries–Timmer–de Vries's 11-country Africa Sector Database and Diao–Harttgen–McMillan's 39-country sample.
2. Build `Reciprocal_c,t` (§6.2) for these countries, and empirically check that it is (near-)absorbing — i.e., that it essentially never reverts from 1 back to 0 for any country in the sample — before committing to a single Callaway–Sant'Anna-style estimator as primary. **Done** — `notebooks/01_clean_trade_agreements.ipynb`; confirmed zero reversions over 1950–2017.
3. Run the McMillan–Rodrik decomposition on the African panel *without* any trade variable first, to replicate de Vries–Timmer–de Vries / Diao–Harttgen–McMillan as a sanity check that the pipeline is correct. **Done** — `notebooks/03_clean_ggdc_productivity.ipynb`: aggregates the GGDC panel's 9 sub-sectors to Agriculture/Manufacturing/Services (§4–§9, following Herrendorf, Rogerson & Valentinyi 2014, confirmed against their Data Appendix), then runs the McMillan & Rodrik (2011) decomposition itself (§10–§14, confirmed against their equation 1 — this caught and fixed a timing error in this document's own earlier statement of the formula, see §6.3). Zero-residual exact decomposition confirmed at annual, full-period, and 3-/5-year interval resolution alike; qualitatively matches the literature's African findings (within-sector dominant and positive for most countries; structural change negative for 4 of 24). Outputs at `data/processed/ggdc_africa_broad_sectors.csv` and the `mcmillan_rodrik_decomposition_*.csv` family (annual, full-period, interval3, interval5, each with a `_by_sector` counterpart).
4. Run the descriptive event-study plots (§6.4.A) — the fastest way to learn whether the design is viable before investing in the heterogeneity or bilateral-trade-weighting extensions. Not yet done.
5. Build the trade- and GDP-weighted exposure variables (§6.2 step 5). **Done** — `notebooks/02_clean_cepii_gravity.ipynb`, using the CEPII Gravity dataset (`data/cepii-gravity/`) as the bilateral trade/GDP source; output merged onto notebook 1's panel as `data/processed/trade_agreements_with_exposure_country_year.csv`. Validated against South Africa's 2000 EU TDCA entry: `reciprocal_trade_share` jumps from 0 to 0.55 exactly the year `Reciprocal_c,t` switches on, rather than drifting gradually — a concrete check that the weighting behaves sensibly, not just that it runs.
6. Merge the productivity and trade-agreement panels into one estimation-ready dataset (§6.2 step 6). **Done** — `notebooks/04_build_estimation_panel.ipynb`; see that step's own entry above for details. Outputs at `data/processed/estimation_panel_interval{3,5}.csv`.
7. Estimate the component regressions (§6.3.A) and the sector-level regressions (§6.4.B/C) with `Reciprocal_c,t` as the sole treatment; compare TWFE vs. staggered-DiD estimates directly, replicating the Sellner–Yotov bias finding in this project's own African sample. Not yet done — this is the next step.
8. Add the agreement-type, sector, and resource-dependence heterogeneity cuts (§6.4.D) and endogeneity checks (§6.4.E/§6.3.B) once the baseline is stable — this is where the ordinal depth score, the FTA-or-deeper cut, and the non-reciprocal-only (AGOA/GSP) comparison from §6.2 actually get used.

---

## 7. Candidate research questions / hypotheses

1. African countries that enter agreements with developed-world partners see faster sectoral labor productivity growth than otherwise-similar African countries that do not, concentrated in manufacturing and tradable services rather than agriculture or mining.
2. The productivity effect increases with agreement depth (services/investment/IP/standards provisions), not just tariff liberalization.
3. A Northern PTA shifts the *composition* of African productivity growth toward the within-sector component and away from the structural-change component — i.e., it induces genuine upgrading in tradable sectors rather than merely reallocating labor toward marginally higher-productivity but low-dynamism sectors (directly testing the "static gains, dynamic losses" pattern documented by de Vries, Timmer & de Vries).
4. The reallocation (structural-change) benefit of a Northern PTA is muted or absent in natural-resource-exporting African countries, consistent with McMillan–Rodrik's "enclave sector" mechanism.
5. Naive TWFE panel estimates of the Northern-PTA effect on African sectoral productivity are biased relative to staggered-adoption-robust estimates, replicating Sellner & Yotov's gravity-context finding in this project's own sample.
6. The productivity payoff to a Northern PTA is larger, and more likely to appear as within-sector upgrading rather than mere reallocation, in African countries with higher initial human capital and institutional quality.

---

## 8. References

Amiti, M., & Konings, J. (2007). Trade Liberalization, Intermediate Inputs, and Productivity: Evidence from Indonesia. *American Economic Review*, 97(5), 1611–1638.

Baier, S. L., & Bergstrand, J. H. (2004). Economic Determinants of Free Trade Agreements. *Journal of International Economics*, 64(1), 29–63.

Baier, S. L., & Bergstrand, J. H. (2007). Do Free Trade Agreements Actually Increase Members' International Trade? *Journal of International Economics*, 71(1), 72–95.

Baier, S. L., Bergstrand, J. H., & Feng, M. (2014). Economic Determinants of Free Trade Agreements Revisited: Distinguishing Sources of Interdependence. *Review of International Economics*, 22(1), 31–58.

Bergstrand, J. H., & Baier, S. L. NSF–Kellogg Institute Database on Economic Integration Agreements. https://sites.nd.edu/jeffrey-bergstrand/database-on-economic-integration-agreements/

Callaway, B., & Sant'Anna, P. H. C. (2021). Difference-in-Differences with Multiple Time Periods. *Journal of Econometrics*, 225(2), 200–230.

Callaway, B., Goodman-Bacon, A., & Sant'Anna, P. H. C. Difference-in-Differences with a Continuous Treatment. NBER Working Paper 32117.

Choudhri, E. U., & Hakura, D. S. International Trade and Productivity Growth: Exploring the Sectoral Effects for Developing Countries. IMF Working Paper.

de Chaisemartin, C., & D'Haultfœuille, X. (2020). Two-Way Fixed Effects Estimators with Heterogeneous Treatment Effects. *American Economic Review*, 110(9), 2964–2996.

De Loecker, J. (2007). Do Exports Generate Higher Productivity? Evidence from Slovenia. *Journal of International Economics*, 73(1), 69–98.

De Loecker, J. (2013). Detecting Learning by Exporting. *American Economic Journal: Microeconomics*, 5(3), 1–21.

de Vries, G., Timmer, M., & de Vries, K. (2015). Structural Transformation in Africa: Static Gains, Dynamic Losses. *The Journal of Development Studies*, 51(6), 674–688.

Diao, X., Harttgen, K., & McMillan, M. (2017). The Changing Structure of Africa's Economies. *The World Bank Economic Review*, 31(2), 412–433. (Also NBER Working Paper 23021 / World Bank Policy Research Working Paper 7958.)

Diao, X., McMillan, M., & Rodrik, D. (2017/2019). The Recent Growth Boom in Developing Economies: A Structural-Change Perspective. NBER Working Paper 23132; reprinted in the *Palgrave Handbook of Development Economics* (2019).

Dür, A., Baccini, L., & Elsig, M. (2014). The Design of International Trade Agreements: Introducing a New Dataset. *The Review of International Organizations*, 9(3), 353–375. Database: https://www.designoftradeagreements.org/

Egger, P. H., Larch, M., & Yotov, Y. V. Gravity-Model Estimation with Time-Interval Data: Revisiting the Impact of Free Trade Agreements.

Groningen Growth and Development Centre & UNU-WIDER. Economic Transformation Database (ETD): content, sources, and methods. University of Groningen. https://www.rug.nl/ggdc/structuralchange/etd/

Herrendorf, B., Rogerson, R., & Valentinyi, Á. (2014). Growth and Structural Transformation. In *Handbook of Economic Growth*, Vol. 2B, Ch. 6, 855–941. Elsevier. (NBER Working Paper 18996.) Source of the Agriculture/Manufacturing/Services sector-aggregation rule used in `notebooks/03_clean_ggdc_productivity.ipynb` — see that notebook and their Data Appendix, pp. 95–96.

Mattoo, A., Rocha, N., & Ruta, M. (Eds.) (2020). *Handbook of Deep Trade Agreements*. World Bank.

Mensah, E. B., & Szirmai, A. (2018). Africa Sector Database (ASD): Expansion and Update. UNU-MERIT Working Paper 2018-020.

McMillan, M., & Rodrik, D. (2011). Globalization, Structural Change and Productivity Growth. NBER Working Paper 17143.

McMillan, M., Rodrik, D., & Verduzco-Gallo, Í. (2014). Globalization, Structural Change, and Productivity Growth, with an Update on Africa. *World Development*, 63, 11–32.

Melitz, M. J. (2003). The Impact of Trade on Intra-Industry Reallocations and Aggregate Industry Productivity. *Econometrica*, 71(6), 1695–1725.

Mulangu, F. M. (2012). Preferential Trade Agreements, Employment, and Productivity: Evidence from AGOA. ACET Working Paper.

Neri-Lainé, M., Orefice, G., & Ruta, M. (2023). Deep Trade Agreements and Heterogeneous Firms' Exports. World Bank Policy Research Working Paper 10277.

Pahl, S., Timmer, M., Gouma, R., & Woltjer, P. (2022). Jobs and Productivity Growth in Global Value Chains: New Evidence for Twenty-Five Low- and Middle-Income Countries. *The World Bank Economic Review*, 36(3), 670–686.

Global value chains and aggregate productivity growth in developing countries: the role of intra-sectoral allocation and structural change. (2024). *Review of World Economics*, 161(1), 89–119.

Sellner, R., & Yotov, Y. V., et al. Staggered Difference-in-Differences in Gravity Settings: Revisiting the Effects of Trade Agreements. *American Economic Journal: Applied Economics*.

Timmer, M. P., de Vries, G. J., & de Vries, K. (2015). Patterns of Structural Change in Developing Countries. GGDC Research Memorandum GD-149, University of Groningen.

World Trade Organization. Regional Trade Agreements Database (RTA-IS). https://rtais.wto.org/

Yotov, Y. V., Piermartini, R., Monteiro, J.-A., & Larch, M. (2016). *An Advanced Guide to Trade Policy Analysis: The Structural Gravity Model*. WTO/UNCTAD.
