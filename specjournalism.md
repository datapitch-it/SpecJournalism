# SpecJournalism — Spec Driven Data Journalism

## What this is

SpecJournalism is a methodology for producing data journalism analyses and statistical reports
using AI as orchestrator. It applies Spec Driven Development principles to data journalism:
the **Story Brief** is the source of truth. Datasets, methodology, charts and narrative are
continuously regenerated outputs.

This framework wraps around the technical execution pipeline of `journoai.md`.
Do not modify `journoai.md`. SpecJournalism adds pre-phases and quality gates
that run before and during the JournAI pipeline.

---

## Core principle

> The journalistic question is fixed first. Data choices serve the question.
> The question never bends to fit available data.

If the available SDMX data cannot answer the Story Brief, the story is paused —
not redefined to fit what is available.

---

## File map

| File | Role | When to load |
|---|---|---|
| `constitution.md` | Immutable rules — checked at every phase | Load first. Always. |
| `brief.md` | Story Brief instructions | Phase SJ-1 |
| `clarify.md` | Structured clarification questions | Phase SJ-2 |
| `null-hypothesis.md` | Falsifiability articulation | Phase SJ-3 |
| `data-design.md` | Methodological plan | Phase SJ-4 |
| `cross-check.md` | Cross-artifact consistency check | Phase SJ-5 (after journoai Phase 3) |
| `tasks.md` | Dependency-ordered execution checklist | Throughout |

---

## Full workflow

### Pre-phases (SpecJournalism)

```
SJ-0  Load constitution.md — verify project constitution exists and is current
SJ-1  Run brief.md         — produce story-brief.md for this analysis
SJ-2  Run clarify.md       — surface underspecified areas, record answers in story-brief.md
SJ-3  Run null-hypothesis.md — articulate falsifiability, record in story-brief.md
SJ-4  Run data-design.md   — produce data-design.md for this analysis
SJ-5  [checkpoint] — do not proceed to JournAI pipeline without SJ-4 complete
```

### Technical execution (JournAI pipeline)

```
Phase 0  Research question — use cleaned version from story-brief.md, do not rewrite
Phase 1  Dataset search    — constrained by data-design.md choices
Phase 2  Data download     — follow journoai.md rules exactly
Phase 3  Inspection        — follow journoai.md rules exactly
[SJ-5 cross-check here]   — run cross-check.md before building the page
Phase 4  Visualisations    — follow journoai.md rules + constitution.md visual rules
Phase 5  HTML page         — follow journoai.md rules exactly
Phase 6  Accountability    — follow journoai.md rules exactly
Phase 7  Executive summary — narrative must answer story-brief.md, not just describe data
```

### Post-execution

```
SJ-6  Pre-publication checklist — journoai.md checklist + specjournalism additions
```

---

## Artefacts produced per analysis

Every analysis in SpecJournalism produces these files in addition to JournAI standard outputs:

```
reports/NN_slug/
├── story-brief.md        ← output of SJ-1 + SJ-2 + SJ-3
├── data-design.md        ← output of SJ-4
├── [standard journoai outputs: index.html, output/, queries/, metadata/, notes.md]
```

`story-brief.md` and `data-design.md` are version-controlled alongside the code they describe.
When the journalistic angle changes, update `story-brief.md` first — then regenerate
`data-design.md` and affected downstream artefacts.

---

## Trigger commands

These commands trigger the corresponding SpecJournalism phase.
Each command loads the relevant file and executes the instructions it contains.

| Command | Loads | Produces |
|---|---|---|
| `/sj.brief` | `brief.md` | `story-brief.md` (what + why, no data) |
| `/sj.clarify` | `clarify.md` | Clarifications section in `story-brief.md` |
| `/sj.nullhyp` | `null-hypothesis.md` | Falsifiability section in `story-brief.md` |
| `/sj.design` | `data-design.md` | `data-design.md` for this analysis |
| `/sj.check` | `cross-check.md` | Cross-check report in `notes.md` |
| `/sj.tasks` | `tasks.md` | Task list printed to terminal |

To run the full pre-phase sequence: `/sj.brief` → `/sj.clarify` → `/sj.nullhyp` → `/sj.design`

---

## What SpecJournalism does NOT replace

- The opensdmx CLI workflow (Phase 1–2 of journoai.md)
- The double-check rule (Phase 2.3 of journoai.md)
- The HTML page structure and initShell() config (Phase 5 of journoai.md)
- The accountability standards and notes.md log (Phase 6 of journoai.md)
- The pre-publication checklist (journoai.md)

SpecJournalism adds structure before and consistency gates during the JournAI pipeline.
It does not rewrite it.

---

## When NOT to use the full pipeline

The full SJ pre-phase sequence (SJ-1 through SJ-4) is designed for:
- New analyses starting from a journalistic angle
- Large feature analyses with multiple datasets
- Investigations where the data selection is non-obvious

**Skip the pre-phases and use journoai.md directly for:**
- Single-dataset extractions with a clear, pre-defined research question
- Updates to existing reports (new data vintage, extended period)
- Technical experiments and dataset explorations with no publication intent

When in doubt, run at minimum `/sj.brief` and `/sj.nullhyp`.
The brief costs five minutes. Skipping it costs a story.
