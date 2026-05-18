# SpecJournalism

**Spec Driven Data Journalism** — a methodology for AI-orchestrated data journalism that applies Spec Driven Development principles to statistical reporting.

> The spec is the source of truth. The plan, the tasks, and the code are continuously regenerated outputs.

SpecJournalism translates this principle from software to journalism: the Story Brief replaces the product spec, and datasets, charts, and narrative replace code as the regenerated outputs.

> The journalistic question is fixed first. Data choices serve the question.  
> The question never bends to fit available data.

---

## How it works

SpecJournalism is **pipeline-agnostic**. It adds pre-phases and quality gates that run before and during any technical execution pipeline — SDMX tools, REST APIs, CSV downloads, database queries, or custom scripts. The execution pipeline is provided by the user; SpecJournalism does not prescribe it.

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

### Technical execution (user-provided pipeline)

After the pre-phases, the user's execution pipeline runs (Phases 0–7). A cross-check gate (`/sj.check`) is inserted after Phase 3 (data inspection) and before Phase 4 (visualisations).

### Post-execution

`/sj.tasks` — prints the dependency-ordered task checklist throughout the process.

---

## Output structure

Each analysis produces:

```
reports/NN_slug/
├── story-brief.md        ← SJ-1 + SJ-2 + SJ-3
├── data-design.md        ← SJ-4
└── [pipeline outputs: index.html, output/, queries/, metadata/, notes.md]
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

Skip pre-phases and go directly to the execution pipeline for:
- Single-dataset extractions with a clear, pre-defined research question
- Updates to existing reports (new data vintage, extended period)
- Technical experiments with no publication intent

When in doubt: run at minimum `/sj.brief` and `/sj.nullhyp`.  
The brief costs five minutes. Skipping it costs a story.

---

## Data access skills

Two skills are available for the dataset discovery and acquisition phases (SJ-4 and pipeline Phase 1):

| Skill | Trigger | Coverage |
|-------|---------|----------|
| `sdmx-explorer` | Statistical data by topic — GDP, unemployment, population, inflation, fertility, energy, etc. | Eurostat, ISTAT, OECD, ECB, World Bank, and other SDMX providers |
| `ckan-mcp` | Open government data portals — "find data on X in country Y" | ~950 CKAN instances worldwide, plus data.europa.eu |

Both skills are invoked automatically when the context matches. They can also be triggered explicitly via `/sdmx-explorer` and `/ckan-mcp`.

Install (if not already present):

```bash
npx skills add -g ondata/opensdmx --skill sdmx-explorer
npx skills add -g ondata/ckan-mcp-server --skill ckan-mcp
```

---

## Background

Spec Driven Development (SDD) was formalized as a workflow for AI coding agents by [GitHub's Spec Kit](https://github.com/github/spec-kit). The core idea: write a structured specification first — focused on the *what* and *why*, not the *how* — and treat all downstream artifacts as regenerated outputs from that spec. The constitution, clarify, and cross-check patterns in SpecJournalism are direct adaptations of Spec Kit's equivalent phases.

SpecJournalism applies the same discipline to data journalism: the Story Brief is the spec, and the data pipeline, visualisations, and narrative are the implementation.
