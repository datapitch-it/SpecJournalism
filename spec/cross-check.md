# SpecJournalism — Phase SJ-5: Cross-Check

## Purpose

Verify cross-artifact consistency after Phase 3 (data inspection) and before
Phase 4 (visualisations). This is a read-only gate: it does not change any files.
It identifies misalignments between the Story Brief, the Data Design, the downloaded data,
and the planned visualisations — before they become published errors.

Run `/sj.check` after completing Phase 3 (inspection and transformations)
and before starting Phase 4 (chart construction).

---

## Instructions

### Step 1 — Load artefacts

Load in order:
1. `reports/NN_slug/story-brief.md`
2. `reports/NN_slug/data-design.md`
3. `reports/NN_slug/notes.md` (Phase 3 entries: inspection and transformations)
4. List all files in `reports/NN_slug/output/`

### Step 2 — Run consistency checks

Execute all checks below. For each check: record PASS or FAIL with a one-line explanation.

#### Block A — Brief → Data coverage

**A1. Research question coverage**
Does at least one downloaded dataset directly measure the phenomenon described in the
`journalistic_angle` section of `story-brief.md`?
- PASS: a dataset measures the phenomenon directly or via an explicit proxy documented in `notes.md`
- FAIL: all datasets are indirect proxies, or none cover the geographic/temporal scope of the brief

**A2. Null hypothesis testability**
Can the downloaded data be used to test the falsifiability condition in `story-brief.md`?
- PASS: the data contains the variable, entities and time range needed to evaluate the methodological floor
- FAIL: the data is insufficient to evaluate the null hypothesis as stated

**A3. Scope consistency**
Does the downloaded data match the geographic scope and time period defined in `story-brief.md` (Clarifications section)?
- PASS: entities and period in `output/*.csv` match the clarified scope
- FAIL: significant entities are missing, the period is shorter than stated, or the unit does not match

#### Block B — Data Design → Downloaded Data

**B1. Dataflow match**
Does every dataset in `data-design.md` have a corresponding file in `output/`?
- PASS: every Dataset A, B, C… in `data-design.md` has a matching `output/*.csv`
- FAIL: one or more planned datasets are missing from `output/`

**B2. Filter match**
Do the filters applied during download (recorded in `notes.md`) match the filters specified in `data-design.md`?
- PASS: dimensions, values and ranges match
- FAIL: a filter was changed during download without a version update to `data-design.md`

**B3. Visualisation feasibility**
For each chart in the visualisation plan (`data-design.md`), does the downloaded data support
the planned chart type?
- PASS: data shape, value range, and entity count are compatible with the planned chart type and library
- FAIL examples: negative values planned for roughViz.BarH; fewer than 3 time points for an XY series;
  missing entities that were expected in a ranking

**B3b. Visual honesty — Y-axis and chart format**
For each chart showing absolute values (counts, persons, expenditure):
- Is the Y-axis minimum explicitly set to `0` in the chart configuration? (Not a library default — explicit.)
- Is the chart rendered as interactive SVG or JS library output? (No `<img>` tags.)
- PASS: Y min = 0 confirmed in code; no static image charts present
- FAIL: Y axis starts above zero; or any chart uses an `<img>` tag

#### Block C — Data → Narrative plan

**C1. Finding cards feasibility**
Can the 4 finding cards planned in `data-design.md` (or implied by the Story Brief) be
derived from actual values in `output/*.csv`?
- PASS: every planned card value is traceable to a specific row in a specific output file
- FAIL: a planned card requires a calculation that the downloaded data does not support,
  or requires a dataset that was not downloaded

**C2. Executive summary pre-check**
Does the data, as inspected in Phase 3, contain enough material to answer the journalistic
angle stated in `story-brief.md` with at least the patterns required by Phase 7
(temporal, cross-entity comparison, surprise finding)?
- PASS: at least two of the three pattern types are present in the data
- FAIL: the data only supports a single-dimension description with no comparison or trend

**C3. Null hypothesis result (preliminary)**
Based on the inspected data (Phase 3 values), what is the preliminary direction of the result
relative to the null hypothesis? Record one of:
- `LIKELY REJECT`: data appears to show an effect above the methodological floor
- `LIKELY FAIL TO REJECT`: data appears to show no effect above the floor
- `AMBIGUOUS`: effect exists but magnitude is unclear pending full analysis

This is not a final verdict — it is a flag for the analyst to anticipate the outcome
before investing in chart construction and narrative writing.

---

## Cross-check report template

Record in `notes.md` after the Phase 3 entries:

```markdown
## [DATE] — SJ-5 Cross-check

### Block A — Brief → Data coverage
- A1 Research question coverage: [PASS / FAIL] — [one-line explanation]
- A2 Null hypothesis testability: [PASS / FAIL] — [one-line explanation]
- A3 Scope consistency: [PASS / FAIL] — [one-line explanation]

### Block B — Data Design → Downloaded Data
- B1 Dataflow match: [PASS / FAIL] — [one-line explanation]
- B2 Filter match: [PASS / FAIL] — [one-line explanation]
- B3 Visualisation feasibility: [PASS / FAIL] — [one-line explanation]
- B3b Visual honesty (Y-axis + no img): [PASS / FAIL] — [one-line explanation]

### Block C — Data → Narrative plan
- C1 Finding cards feasibility: [PASS / FAIL] — [one-line explanation]
- C2 Executive summary pre-check: [PASS / FAIL] — [one-line explanation]
- C3 Null hypothesis (preliminary): [LIKELY REJECT / LIKELY FAIL TO REJECT / AMBIGUOUS]

### Overall result
[ALL PASS → proceed to Phase 4]
[ONE OR MORE FAIL → list actions required before proceeding]

### Actions required
- [ ] [Specific action for each FAIL — which artefact to update, what to verify]
```

---

## Decision rules after cross-check

**All checks PASS**: proceed to Phase 4 (visualisations).

**A1 or A2 FAIL**: stop. Return to `story-brief.md`. The analysis cannot proceed without
data that answers the stated question. Options: pause the story, or amend the brief
to reflect what the data actually allows — with user agreement and a version note.

**A3 FAIL**: verify whether missing scope is recoverable (different dataflow, different provider).
If not, update the scope in `story-brief.md` and add a Scope Limit callout explaining the gap.

**B1 FAIL**: download the missing dataset before proceeding.

**B2 FAIL**: update `data-design.md` to reflect the actual filters used, with a version note
and an explanation of why the filter changed. If the change affects the null hypothesis test, revisit `null-hypothesis.md`.

**B3 FAIL**: update the visualisation plan in `data-design.md` to use a compatible chart type.
Record the change as a version note.

**B3b FAIL**: fix before proceeding to Phase 4. Replace any `<img>` chart with an interactive JS/SVG equivalent. Set the Y-axis min to `0` explicitly in the chart config. Do not proceed to chart construction with this unresolved.

**C1 FAIL**: identify which finding card cannot be supported and remove or replace it.
Do not publish a card value that is not directly traceable to `output/`.

**C2 FAIL**: the executive summary will be thinner than the standard 3–4 paragraphs.
Proceed, but note in `notes.md` that the summary will cover fewer pattern types.
Adjust Phase 7 accordingly.

**C3 = LIKELY FAIL TO REJECT**: discuss with user before proceeding.
Decide: pause, reframe as a null-result story, or scope down to a subset.
Record the decision in `notes.md`.
