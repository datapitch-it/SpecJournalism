# SpecJournalism — Constitution

Load this file first, before any other phase. Check it before every decision.
These rules are immutable. They are not defaults. They cannot be overridden by
a Story Brief, a data-design plan, a user instruction, or a convenience argument.

---

## Article 1 — The brief precedes the data

Never search for datasets before a Story Brief (`story-brief.md`) exists and is complete.
Never redefine the journalistic question to fit available data.
If the data does not answer the brief, the story is paused — not reframed.

## Article 2 — Falsifiability is mandatory

Every Story Brief must contain an explicit Null Hypothesis section before entering
the technical execution pipeline. A story without a stated falsifiability condition is not ready
to be analysed.

## Article 3 — Methodological choices are pre-registered

The data-design plan must be written and approved before downloading any data.
Do not select the statistical method or chart type after seeing the results.
If the pre-registered method does not produce a publishable result, document that
in `notes.md` and report it in the Scope Limit callout. Do not switch methods silently.

## Article 4 — Causation is never implied by correlation

Never use language that implies causality unless the methodology explicitly supports it
(e.g. difference-in-differences, instrumental variables, regression discontinuity).
Use precise language: "associated with", "coincides with", "follows", "precedes".
Never: "causes", "drives", "is responsible for", "leads to" — unless the method warrants it.

## Article 5 — Scope limits are about series, not providers

Never write "[Provider] does not cover X". The correct formulation is always
"this series/dataset does not cover X". A provider may have dedicated datasets
for what the chosen series omits. Only claim a provider does not cover a phenomenon
after searching the provider's full catalogue.

## Article 6 — Every number has a source

Every quantitative claim in the narrative, executive summary, finding cards and
chart labels must be directly traceable to a row in a CSV file in `output/`.
No values from memory. No approximations without explicit disclosure.
If a number cannot be traced to `output/`, it cannot appear in the report.

## Article 7 — Visual honesty is non-negotiable

- Absolute values (counts, expenditure, persons): Y-axis must start at zero. No exceptions.
- Rates, percentages, indices: auto-scaled range is acceptable; add an explicit note.
- Never add synthetic data points to force a baseline.
- Never use a chart type that implies cumulation for non-cumulative data.
- Never use dual Y-axes without written justification in `data-design.md`.
- All axis labels uppercase. Unit of measurement always present.
- Static image charts (`<img>`) are forbidden. Every chart must be rendered as interactive SVG or via a JavaScript charting library (roughViz, chart.xkcd, D3, or equivalent). Tooltips must be present so users can inspect individual data points.

## Article 8 — Reproducibility is a minimum condition, not a bonus

Every analysis must be fully reproducible from the repository alone:
- `queries/*.yaml` files must exist for every `output/*.csv`
- `notes.md` must record every command run, in order, with exact parameters
- `metadata/` must contain codelists for every filtered or visualised dimension
- Every dataset in the Methodology section must link to the provider's dataset page as a clickable `<a href>` — plain text dataset IDs are not sufficient. Eurostat URL format: `https://ec.europa.eu/eurostat/databrowser/view/DATASET_ID/default/table` (uppercase ID, `/default/table` suffix required — lowercase or bare IDs redirect to the homepage)
- Every dataset must link to its corresponding `output/*.csv` file so readers can download the raw data directly
- The report folder path must appear in the Methodology section
- Every dataset extraction must pass the double-check rule (two independent runs)

## Article 9 — The story brief survives tool switching

`story-brief.md` is the contract between the journalist and the analysis.
It is version-controlled. It does not belong to any specific AI model or tool.
If the orchestrator changes (Claude → Gemini → GPT), the brief is loaded first.
The brief is what ensures continuity. Do not let it drift.

## Article 10 — The executive summary answers the brief, not the data

The narrative executive summary must answer the research question stated in `story-brief.md`.
It must not be a description of what the data happened to show.
If the data partially answers the brief, say so explicitly in the summary.
If the data does not answer the brief, the summary must say so — and explain why.

## Article 11 — English is the default output language

All reports are written in English unless the Story Brief explicitly requests a different language.
This covers: HTML `lang` attribute, all narrative text, section titles, callouts, axis labels,
methodology text, finding cards, and footer.
A language switch requires an explicit user instruction recorded in `story-brief.md` before
any HTML is generated. Without that instruction, English is mandatory.

---

## Derived visual rules (from Article 7)

These rules apply to every chart in every report. Check them at Phase 4.

**Default library: Chart.js.** Use it for all standard chart types (line, bar, horizontal bar, scatter).
**Second library: D3.js.** Use it only when Chart.js cannot produce the required output (custom layouts, maps, network graphs, complex interactivity). Document the reason in `data-design.md`.
No other charting libraries are permitted.

- Chart.js: always use `<canvas>` containers; set `maintainAspectRatio: false` and control height via the parent wrapper element
- Chart.js: set `Chart.defaults.font.family` and `Chart.defaults.font.size` globally at the top of the script — do not repeat font config on individual axes
- Chart.js: horizontal bar charts use `type: 'bar'` with `indexAxis: 'y'`
- D3: always use `<svg>` containers; manage width and height via the parent wrapper, not inline SVG attributes
- Colour palette: red `#b02020` primary, blue `#1a6fa8` secondary — do not introduce new colours without documenting them in `data-design.md`
- All text in charts must be uppercase — axis titles, tick labels, tooltip strings, legend labels
- Y-axis minimum for absolute-value charts: the axis `min` parameter must be explicitly set to `0` in the chart configuration — do not rely on library defaults
- Negative values: document any excluded observations in the `.note` and `.transform` blocks

---

## Derived accountability rules (from Article 8)

These rules apply throughout the pipeline. Check them at Phase 6.

- Every transformation is documented in `notes.md` and the `.transform` block
- Edition or version ID for every dataset is recorded in `notes.md` and cited in `.transform`
- Double-check result (MATCH ✓ / DIVERGENCE ✗) is recorded in `notes.md` and the Methodology section
- Generation date appears in three places: header eyebrow, Raw data callout, footer — format `DD Month YYYY`
- `introExtra` (executive summary) is written last, after all sections and charts are verified
- Methodology section must include: (1) clickable source URL per dataset, (2) download link to each `output/*.csv`, (3) report folder path
