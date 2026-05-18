# Notes — Migrant arrivals to Italy over time

## Phase 1 — Dataset acquisition (18 May 2026)

### Commands run (in order)

```bash
# Dataset A — Total immigration flows, all EU countries
opensdmx get MIGR_IMM1CTZ --citizen TOTAL --agedef COMPLET --age TOTAL --unit NR --sex T \
  --out output/A_immigration_flows.csv \
  --query-file queries/A_immigration_flows.yaml

# Dataset B1 — Asylum applicants historical series (1985–2007)
opensdmx get MIGR_ASYCTZ --citizen TOTAL --unit PER \
  --out output/B1_asylum_historical.csv \
  --query-file queries/B1_asylum_historical.yaml

# Dataset B2 — Asylum applicants current series (2008–2025)
opensdmx get MIGR_ASYAPPCTZA --citizen TOTAL --applicant TOTAL --sex T --age TOTAL \
  --out output/B2_asylum_current.csv \
  --query-file queries/B2_asylum_current.yaml

# Dataset C — Non-EU residence permit inflows, Italy national total
opensdmx get 29_348_DF_DCIS_PERMSOGG1_11 --provider istat \
  --REF_AREA IT --SEX 9 --AGE TOTAL \
  --out output/C_residence_permits.csv \
  --query-file queries/C_residence_permits.yaml

# Dataset D — Total population denominator
opensdmx get DEMO_PJAN --age TOTAL --sex T \
  --out output/D_population.csv \
  --query-file queries/D_population.yaml
```

### Deviation from data-design.md

**Dataset C**: data-design.md specified `29_348_DF_DCIS_PERMSOGG1_8` (by citizenship,
reason, sex, age). Switched to `29_348_DF_DCIS_PERMSOGG1_11` (by sex, age, province)
because the `_8` dataset has no TOTAL code for citizenship — `999` = "Stateless" (not
total). The `_11` dataset uses `MOSTREL_CCITENSHIP=WORLD` which is the correct
national aggregate. Data-design.md updated to v1.1.

**Dataset B2**: data-design.md specified `TPS00191`. Switched to `MIGR_ASYAPPCTZA`
because TPS00191 starts from 2014 for Italy, leaving a 2008–2013 gap. `MIGR_ASYAPPCTZA`
starts from 2008 and bridges cleanly with B1. Data-design.md updated to v1.1.

### Row counts and time ranges

| File | Rows | Italy range | Countries |
|---|---|---|---|
| A_immigration_flows.csv | 852 | 1998–2024 | 49 |
| B1_asylum_historical.csv | 542 | 1985–2007 | 23 |
| B2_asylum_current.csv | 584 | 2008–2025 | 34 |
| C_residence_permits.csv | 18 | 2007–2024 | Italy only |
| D_population.csv | 3027 | 1960–2025 | 59 |

### Double-check results

| Dataset | Check value | Result |
|---|---|---|
| A: Italy 2024 immigration | 451,583 persons | MATCH ✓ |
| B2: Italy 2025 asylum | 133,350 persons | MATCH ✓ |
| B1: Italy 2007 asylum (last) | 14,055 persons | MATCH ✓ |

### Notable observations at inspection

- **B1/B2 join**: B1 ends at 2007 (14,055), B2 starts at 2008 (30,145). Gap values
  in B2: 2008=30,145; 2009=17,755; 2010=10,050; 2011=40,350 (Arab Spring spike).
  The join is clean — no overlap. Series break must be annotated on chart V4.

- **Dataset A — Italy 2003 spike**: Italy 2003 shows 440,301 arrivals vs. 213,202
  in 2002 — a near-doubling. This likely reflects a regularisation amnesty
  (sanatoria) under Berlusconi II. Context note required on chart V1.

- **Dataset C coverage**: only 2007–2024, confirming the data-design note. The 2020
  drop (106,503) is visible — COVID pandemic border closures.

- **B1 — 1991 spike**: 24,490 asylum applicants in Italy in 1991 vs. 3,570 in 1990.
  Reflects Albanian migration crisis (collapse of communist regime). Context note
  required on chart V4.

- **No OBS_FLAG issues** found in Italian series for Datasets A, B1, B2. Dataset D
  has no flags for Italy.

### Key values computed at inspection

Italy immigration per 100k (Dataset A + D):

| Year | Arrivals | Per 100k | Note |
|---|---|---|---|
| 1998 | 156,885 | 276 | Series start |
| 2003 | 440,301 | 770 | Regularisation spike (Berlusconi II) |
| 2007 | 527,123 | 901 | Pre-crisis peak |
| 2008 | 534,712 | 906 | **All-time series maximum** |
| 2011 | 385,793 | 644 | Post-crisis fall |
| 2015 | 280,078 | 465 | Asylum crisis peak, but immigration lower |
| 2020 | 247,526 | 415 | COVID drop |
| 2024 | 451,583 | 766 | Recent high — below 2007–2008 |

**Key finding**: Italy's immigration peak (absolute and per capita) occurred in 2007–2008,
not in recent years. 2024 is the highest since 2012 but 15% below the all-time high.
This directly challenges "unprecedented emergency" framing in current political debate.

Asylum applicants Italy (Dataset B1+B2 joined):

| Year | Applicants | Note |
|---|---|---|
| 1985 | 5,400 | Series start |
| 1991 | 24,490 | Albanian crisis spike |
| 1996 | 680 | All-time series minimum |
| 2011 | 40,350 | Arab Spring |
| 2016 | 122,960 | Mediterranean crisis peak |
| 2017 | 128,855 | Peak under Gentiloni |
| 2020 | 26,950 | COVID drop |
| 2024 | 158,605 | **Asylum all-time series maximum** |
| 2025 | 133,350 | Slight decline |

**Key finding**: asylum applications in 2024 ARE at an all-time series high (158,605)
— a distinct trend from total immigration. The asylum series and immigration series
diverge significantly: immigration is below its 2008 peak, but asylum applications are
at a record. This distinction is editorially essential.

---

## 18 May 2026 — SJ-5 Cross-check

### Block A — Brief → Data coverage
- A1 Research question coverage: PASS — MIGR_IMM1CTZ directly measures immigration arrivals; B1+B2 extends asylum portrait to 1985
- A2 Null hypothesis testability: PASS — descriptive analysis; framing commitments (full series, no cherry-picking) are verifiable
- A3 Scope consistency: PASS — 49 EU countries and Italy from 1998 (immigration) / 1985 (asylum); exceeds 20-year methodological floor

### Block B — Data Design → Downloaded Data
- B1 Dataflow match: PASS — all 5 datasets have output files
- B2 Filter match: PASS WITH NOTES — 2 deviations documented in data-design.md v1.1 and notes.md before this check
- B3 Visualisation feasibility: PASS — all chart types compatible with data shape; no negative values; V4 has 41 points; join clean at 2007/2008

### Block C — Data → Narrative plan
- C1 Finding cards feasibility: PASS — all key values traceable to specific rows in output files
- C2 Executive summary pre-check: PASS — temporal trend, cross-EU comparison, and surprise finding (peak in 2007–2008, not 2022–2024) all present
- C3 Null hypothesis (preliminary): AMBIGUOUS — descriptive analysis; data is rich and supports a clear narrative with multiple distinct findings

### Overall result
ALL PASS → proceed to Phase 4 (visualisations)

### Phase 4 — Chart library deviation

data-design.md specified chart.xkcd XY for V1, V4, V5. These were switched to
matplotlib-generated PNGs because chart.xkcd XY does not support a forced y=0 baseline —
its y-scale is data-driven with no `yMinValue` option. Since V1, V4, V5 show absolute
counts (Article 7: y-axis must start at zero for absolute values), matplotlib was used
instead. data-design.md updated to v1.2.

V2 (immigration rate per 100k) and V3 (EU ranking, roughViz BarH) remain as JS charts.
V2 is a rate — auto-scaled y-axis is acceptable per Article 7 with an explicit note.

### Key editorial alert for Phase 7
The immigration series and asylum series tell **different stories**:
- Total immigration: peak was 2007–2008; current levels are elevated but not record-breaking
- Asylum applications: 2024 IS a record high (158,605)
Both findings must appear in the executive summary. Collapsing them would misrepresent the data.
