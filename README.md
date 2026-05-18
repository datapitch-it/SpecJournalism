# SpecJournalism

**Spec Driven Data Journalism** — a methodology for AI-orchestrated data journalism that applies Spec Driven Development principles to statistical reporting.

> The spec is the source of truth. The plan, the tasks, and the code are continuously regenerated outputs.

SpecJournalism translates this principle from software to journalism: the Story Brief replaces the product spec, and datasets, charts, and narrative replace code as the regenerated outputs.

> The journalistic question is fixed first. Data choices serve the question.  
> The question never bends to fit available data.

---

## How it works

SpecJournalism wraps around the `journoai.md` technical pipeline by adding pre-phases and quality gates that run before and during execution.

### Pre-phases

| Phase | Command | Output |
|-------|---------|--------|
| SJ-0 | — | Load `constitution.md` |
| SJ-1 | `/sj.brief` | `story-brief.md` (what + why, no data) |
| SJ-2 | `/sj.clarify` | Clarifications section in `story-brief.md` |
| SJ-3 | `/sj.nullhyp` | Falsifiability section in `story-brief.md` |
| SJ-4 | `/sj.design` | `data-design.md` for this analysis |
| SJ-5 | checkpoint | Do not proceed without SJ-4 complete |

Full pre-phase sequence: `/sj.brief` → `/sj.clarify` → `/sj.nullhyp` → `/sj.design`

### Technical execution

After the pre-phases, the standard JournAI pipeline runs (Phases 0–7), with a cross-check gate (`/sj.check`) inserted before building the final page.

### Post-execution

`/sj.tasks` — prints the dependency-ordered task checklist throughout the process.

---

## Output structure

Each analysis produces:

```
reports/NN_slug/
├── story-brief.md        ← SJ-1 + SJ-2 + SJ-3
├── data-design.md        ← SJ-4
└── [standard journoai outputs: index.html, output/, queries/, metadata/, notes.md]
```

`story-brief.md` and `data-design.md` are version-controlled alongside the code they describe. When the journalistic angle changes, update `story-brief.md` first, then regenerate downstream artefacts.

---

## File map

| File | Role |
|------|------|
| `spec/constitution.md` | Immutable rules — checked at every phase |
| `spec/brief.md` | Story Brief instructions |
| `spec/clarify.md` | Structured clarification questions |
| `spec/null-hypothesis.md` | Falsifiability articulation |
| `spec/data-design.md` | Methodological plan |
| `spec/cross-check.md` | Cross-artifact consistency check |
| `spec/tasks.md` | Dependency-ordered execution checklist |
| `spec/status.md` | Project status |
| `spec/specjournalism.md` | Full methodology reference |

---

## When to skip the pre-phases

The full SJ sequence (SJ-1 through SJ-4) is for:
- New analyses starting from a journalistic angle
- Large feature analyses with multiple datasets
- Investigations where data selection is non-obvious

Skip pre-phases and use `journoai.md` directly for:
- Single-dataset extractions with a clear, pre-defined research question
- Updates to existing reports (new data vintage, extended period)
- Technical experiments with no publication intent

When in doubt: run at minimum `/sj.brief` and `/sj.nullhyp`.  
The brief costs five minutes. Skipping it costs a story.

---

## Background

Spec Driven Development (SDD) was formalized as a workflow for AI coding agents by [GitHub's Spec Kit](https://github.com/github/spec-kit). The core idea: write a structured specification first — focused on the *what* and *why*, not the *how* — and treat all downstream artifacts as regenerated outputs from that spec. The constitution, clarify, and cross-check patterns in SpecJournalism are direct adaptations of Spec Kit's equivalent phases.

SpecJournalism applies the same discipline to data journalism: the Story Brief is the spec, and the data pipeline, visualisations, and narrative are the implementation.
