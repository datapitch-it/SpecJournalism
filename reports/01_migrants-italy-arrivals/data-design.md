# Data Design — Migrant arrivals to Italy over time
Linked to: story-brief.md v1.0 dated 18 May 2026

## Methodological approach
Three complementary datasets are combined to produce a long-run portrait of migration
to Italy within a European context: total immigration flows (1998–2024), asylum
applications (1985–2024, two series joined), and non-EU residence permit inflows
(from ISTAT, shorter coverage). Italy is the primary subject; European data from the
same Eurostat sources provides the comparative backdrop. Both absolute counts and
rates per 100,000 population are produced; rates require a separate population
denominator series. The Italian PM timeline (1994 onwards) is annotated visually
on the Italian time series charts — no causal claim is made.

---

## Datasets

### Dataset A — Total immigration flows (Italy + Europe)
- Source: Eurostat
- Dataset ID: `MIGR_IMM1CTZ`
- Dimensions / filters:
  - `freq`: A (annual)
  - `citizen`: TOTAL (all citizenships)
  - `agedef`: COMPLET
  - `age`: TOTAL
  - `unit`: NR (number of persons)
  - `sex`: T (total)
  - `geo`: IT + all available EU/EEA countries (49 entities)
- Time range: 1998–most recent available (confirmed: 2024 for Italy)
- Geographic scope: Italy primary; all 49 Eurostat reporting entities as backdrop
- Unit: absolute count (persons); rate per 100,000 population derived in post-processing
- Output file: `output/A_immigration_flows.csv`
- Query file: `queries/A_immigration_flows.yaml`
- Covers: annual immigration to Italy and EU countries — the main long-run flow metric

### Dataset B1 — Asylum applicants, historical (1985–2007)
- Source: Eurostat
- Dataset ID: `MIGR_ASYCTZ`
- Dimensions / filters:
  - `citizen`: TOTAL
  - `unit`: PER
  - `geo`: IT (and available EU countries)
- Time range: 1985–2007
- Geographic scope: Italy; EU countries where available
- Unit: number of persons (applicants)
- Output file: `output/B1_asylum_historical.csv`
- Query file: `queries/B1_asylum_historical.yaml`
- Covers: asylum applications in Italy — historical series, pre-2008

### Dataset B2 — Asylum applicants, current (2008–present)
- Source: Eurostat
- Dataset ID: `MIGR_ASYAPPCTZA` (changed from `TPS00191` at Phase 1 — see notes.md)
- Dimensions / filters:
  - `citizen`: TOTAL
  - `applicant`: TOTAL
  - `sex`: T
  - `age`: TOTAL
- Time range: 2008–most recent available
- Geographic scope: Italy primary; EU countries
- Unit: number of persons (applicants)
- Output file: `output/B2_asylum_current.csv`
- Query file: `queries/B2_asylum_current.yaml`
- Covers: asylum applications in Italy — current series, 2008 onwards
- Note: B1 and B2 are joined in post-processing to produce a single 1985–2024 series.
  A series-break flag must be documented at 2007/2008 junction in `notes.md`.

### Dataset C — Non-EU residence permit inflows (Italy only)
- Source: ISTAT
- Dataset ID: `29_348_DF_DCIS_PERMSOGG1_11` (changed from `_8` at Phase 1 — see notes.md)
- Dimensions / filters:
  - `REF_AREA`: IT
  - `DATA_TYPE`: INFLNONEU (inflows, non-EU citizens)
  - `SEX`: 9 (total)
  - `AGE`: TOTAL
  - citizenship: TOTAL (where available)
- Time range: earliest available–most recent (confirmed start approx. 2013)
- Geographic scope: Italy only
- Unit: number of permits issued
- Output file: `output/C_residence_permits.csv`
- Query file: `queries/C_residence_permits.yaml`
- Covers: new residence permits granted to non-EU citizens — a distinct policy-relevant
  metric (EU citizens move freely and do not appear in this series)

### Dataset D — Population denominator (for rates)
- Source: Eurostat
- Dataset ID: to be identified at Phase 1 — candidate: `DEMO_PJAN` (population on 1 January)
- Dimensions / filters: `geo` = IT + EU countries; `sex` = T; `age` = TOTAL
- Time range: matching Datasets A and B
- Geographic scope: Italy + EU countries
- Unit: number of persons
- Output file: `output/D_population.csv`
- Query file: `queries/D_population.yaml`
- Covers: total resident population — denominator for per-100k rate calculation

---

## Visualisation plan

| Section ID | Dataset | Description | Chart type | Library | Entities | Period |
|---|---|---|---|---|---|---|
| V1 | A | Italy total immigration — long run, absolute | XY line | chart.xkcd | IT | 1998–2024 |
| V2 | A | Italy immigration rate per 100k vs. EU average | XY line | chart.xkcd | IT + EU avg | 1998–2024 |
| V3 | A | Most recent year: immigration rate ranking, EU countries | BarH | roughViz | ~30 countries | latest year |
| V4 | B1+B2 | Asylum applications in Italy — joined series 1985–2024 | XY line | chart.xkcd | IT | 1985–2024 |
| V5 | C | Non-EU residence permit inflows, Italy | XY line | chart.xkcd | IT | available range |

**Political annotation**: V1 and V4 will carry labelled vertical bands for Italian PM
periods (1994 onwards for V1, full available range for V4). Annotation is visual only;
no analytical claim is made about the relationship between government and flows.

**Dual Y-axis**: not planned. The asylum series (V4) and immigration series (V1) are
presented as separate charts, not overlaid on dual axes, to avoid implying a direct
proportional relationship between the two phenomena.

**Negative values**: not expected in any dataset. roughViz.BarH is safe to use for V3.

---

## Data quality risks

- **Dataset A (MIGR_IMM1CTZ)**: Coverage before 2008 is thinner for some Eastern European
  countries (EU enlargement in 2004/2007 affects comparability). Italy data starts 1998
  and appears continuous. Some years may carry `OBS_FLAG = b` (series break) — inspect
  at Phase 2 and document in `notes.md`.

- **Dataset B1 (MIGR_ASYCTZ)**: Historical series — collection methodology differs from
  post-2008 data. Absolute values are less reliable for early years (pre-1995). The 1991
  spike (24,490) reflects Albanian arrivals — a genuine event, not a data artefact, but
  context note required in report.

- **Dataset B1+B2 join**: The 2007/2008 junction is a methodological break. Both series
  must be shown at the join point and the break disclosed in the Scope Limit callout.

- **Dataset C (ISTAT residence permits)**: Short coverage (~2013 onwards). Non-EU only —
  EU citizens are structurally absent. This is a policy instrument count, not a migration
  flow count; the difference must be explained inline for a general audience.

- **Dataset D (population denominator)**: If the population year does not exactly match
  the immigration flow year, document the offset in `notes.md`. Use mid-year population
  where available; 1 January population otherwise (note the assumption).

---

## Methodological commitments

Pre-registered before any data download. Changes after download must be documented
in `notes.md` with the reason.

- **Full time series shown**: the complete available range for each dataset is presented,
  not a window selected after viewing the data.
- **Political annotation**: visual bands only; no regression, no period comparison,
  no statistical test between government periods.
- **Rate calculation**: immigration count divided by total resident population × 100,000.
  Denominator source: Dataset D. If denominator is unavailable for a given year,
  that year is excluded from the rate chart and the gap is disclosed.
- **Series join (B1+B2)**: the join is applied at the 2007/2008 boundary. No
  interpolation or smoothing is applied at the join. A visible break marker appears
  on V4.
- **EU average for V2**: unweighted average across countries with data in a given year,
  unless a population-weighted average is clearly more appropriate — decision recorded
  at Phase 3 after inspecting country coverage gaps.

---

## Version history
- 18 May 2026 v1.0 — initial design
- 18 May 2026 v1.1 — Dataset B2 changed from TPS00191 to MIGR_ASYAPPCTZA (coverage gap 2008–2013); Dataset C changed from _8 to _11 (no TOTAL citizenship code in _8)
