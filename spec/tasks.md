# SpecJournalism — Task Checklist

## Purpose

Dependency-ordered execution checklist for a SpecJournalism analysis.
Tasks marked `[P]` can run in parallel with adjacent `[P]` tasks.
Each checkpoint must pass before the next block begins.

This checklist is printed at the start of every analysis and updated as tasks complete.
Record completion in `notes.md` with date and any anomalies found.

---

## Pre-phase block (SpecJournalism)

- [ ] SJ-0 — Load `constitution.md` and confirm it is the current version
- [ ] SJ-1 — Run `/sj.brief` → produce `story-brief.md`
- [ ] SJ-2 — Run `/sj.clarify` → fill `Clarifications` section in `story-brief.md`
- [ ] SJ-3 — Run `/sj.nullhyp` → fill `Null hypothesis` section in `story-brief.md`
- [ ] SJ-4 — Run `/sj.design` → produce `data-design.md`

**CHECKPOINT A**: `story-brief.md` complete (angle, hook, scope, clarifications, null hypothesis) AND `data-design.md` complete (datasets, filters, visualisation plan, methodological commitments). Do not enter the execution pipeline until both files exist.

---

## Phase 0 — Research question

- [ ] 0.1 — Write cleaned research question in `notes.md` (source: `story-brief.md` angle)
- [ ] 0.2 — Write statistical proxy in `notes.md` (source: `data-design.md` dataset choices)
- [ ] 0.3 — Record candidate data sources in `notes.md`

**Note**: the research question must be derived from `story-brief.md`. Do not rewrite the scope.

---

## Phase 1 — Dataset search

- [ ] 1.1 [P] — Verify [DATASET_A] — confirm dimensions, coverage and availability
- [ ] 1.2 [P] — Verify [DATASET_B] — confirm dimensions, coverage and availability
- [ ] 1.3 [P] — Download metadata for every filtered dimension into `metadata/`
- [ ] 1.4 — Confirm dataset IDs in `data-design.md` against actual availability
  - If a dataset ID is wrong or unavailable: update `data-design.md` v[X.X+1] before proceeding

---

## Phase 2 — Data acquisition

- [ ] 2.1 — Acquire [DATASET_A] with filters from `data-design.md` → `output/A_[slug].csv`
- [ ] 2.2 — Run double-check for Dataset A (alternative method or second independent run)
- [ ] 2.3 — Save `queries/A_[slug].yaml` (or equivalent query record)
- [ ] 2.4 [P] — Acquire [DATASET_B] → `output/B_[slug].csv`
- [ ] 2.5 [P] — Run double-check for Dataset B
- [ ] 2.6 [P] — Save `queries/B_[slug].yaml`
- [ ] 2.N — [repeat for each additional dataset]
- [ ] 2.X — Record edition/version ID for every dataset in `notes.md`

**CHECKPOINT B**: every `output/*.csv` has a matching `queries/*.yaml` and a passed double-check entry in `notes.md`.

---

## Phase 3 — Inspection and transformations

- [ ] 3.1 [P] — Inspect `output/A_[slug].csv`: rows, columns, flags, missing values
- [ ] 3.2 [P] — Inspect `output/B_[slug].csv`
- [ ] 3.3 — Document every transformation in `notes.md` and plan `.transform` block content
- [ ] 3.4 — Verify that transformed data supports the visualisation plan in `data-design.md`

---

## Cross-check gate (SpecJournalism SJ-5)

- [ ] SJ-5 — Run `/sj.check` → record cross-check report in `notes.md`
- [ ] SJ-5a — All Block A checks PASS (or FAIL with agreed remediation)
- [ ] SJ-5b — All Block B checks PASS (or FAIL with agreed remediation)
- [ ] SJ-5c — All Block C checks PASS (or FAIL with agreed remediation)
- [ ] SJ-5d — C3 result recorded (LIKELY REJECT / LIKELY FAIL TO REJECT / AMBIGUOUS)

**CHECKPOINT C**: cross-check report complete with no unresolved FAILs. Do not build charts until this checkpoint passes.

---

## Phase 4 — Visualisations

- [ ] 4.1 [P] — Build chart for Dataset A section (type and library from `data-design.md`)
- [ ] 4.2 [P] — Build chart for Dataset B section
- [ ] 4.N [P] — [repeat for each additional dataset section]
- [ ] 4.X — Verify all charts render without console errors
- [ ] 4.Y — Verify all axis labels are uppercase and units are present
- [ ] 4.Z — Verify Y-axis baseline rule (zero for absolutes, auto-scale for rates with note)

---

## Phase 5 — Publication page

- [ ] 5.1 — Write HTML sections for each dataset (section-label, subtitle, chart-wrap, transform, note)
- [ ] 5.2 — Configure `initShell()` — all `_en` variants present for bilingual toggle
- [ ] 5.3 — Verify sticky nav links resolve to correct section IDs
- [ ] 5.4 — Verify generation date appears in three places (header, raw data callout, footer)
- [ ] 5.5 — Update `reports.json` with new entry

---

## Phase 6 — Accountability

- [ ] 6.1 — `notes.md` contains all phases with exact commands and dates
- [ ] 6.2 — Every dataset section has `.transform` block (even if "no transformations applied")
- [ ] 6.3 — Every dataset section has `.note` block with reading instructions and source
- [ ] 6.4 — Methodology section: data source URLs verified; CLI commands or API calls recorded
- [ ] 6.5 — Double-check PASS blocks present in Methodology section for every dataset
- [ ] 6.6 — Raw data section: CSV download links, rows, period, extraction date, licence

---

## Phase 7 — Executive summary

- [ ] 7.1 — Run pattern inventory (temporal, cross-entity, surprise) and record in `notes.md`
- [ ] 7.2 — Write 3–4 paragraph executive summary in `introExtra`
- [ ] 7.3 — Verify every claim cites an exact value with year traceable to `output/`
- [ ] 7.4 — Verify every cited section has a working `<a href="#section-id">` link
- [ ] 7.5 — **Verify the summary answers `story-brief.md` journalistic angle** — not just describes data
  - If the null hypothesis was not rejected: the summary must say so explicitly

---

## Pre-publication checklist (combined)

### SpecJournalism additions
- [ ] `story-brief.md` is present in the analysis folder and version-controlled
- [ ] `data-design.md` is present in the analysis folder and version-controlled
- [ ] The research question in the report matches `story-brief.md` (scope and intent unchanged)
- [ ] The executive summary answers the journalistic angle, not a convenient subset of the data
- [ ] If C3 was LIKELY FAIL TO REJECT: the null result is disclosed in the Scope Limit callout
- [ ] No methodological commitment was changed after data download without a `data-design.md` version note

### Execution pipeline checklist
[Run the full pre-publication checklist from the chosen execution pipeline]

---

## How to use this checklist

At the start of each analysis, copy this file into `reports/NN_slug/tasks.md`.
Replace `[DATASET_A]`, `[DATASET_B]` etc. with the actual dataset IDs or names from `data-design.md`.
Add rows for each additional dataset.
Mark tasks as complete (`[x]`) as you go.
Do not mark a checkpoint complete unless all tasks in its block are checked.
