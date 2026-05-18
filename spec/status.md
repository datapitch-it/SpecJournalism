# SpecJournalism — Project Status

## What it is

SpecJournalism is a Spec Driven Data Journalism framework that applies SDD (Spec Driven Development)
principles to the production of AI-orchestrated data journalism analyses.

It adds pre-phases (Story Brief, Clarify, Null Hypothesis, Data Design) and cross-artifact
consistency gates (Cross-Check) that run before and during any technical execution pipeline.
SpecJournalism is pipeline-agnostic.

The journalistic question is fixed first. Data choices serve the question. The question
never bends to fit available data.

## Stack

- `specjournalism.md` — orchestrator, defines the full workflow and trigger commands
- `constitution.md` — immutable rules, loaded first at every phase
- `brief.md` — Phase SJ-1: Story Brief instructions
- `clarify.md` — Phase SJ-2: structured clarification questions
- `null-hypothesis.md` — Phase SJ-3: falsifiability articulation
- `data-design.md` — Phase SJ-4: methodological plan
- `cross-check.md` — Phase SJ-5: cross-artifact consistency gate
- `tasks.md` — dependency-ordered execution checklist

Requires: a technical execution pipeline (Phases 0–7) provided by the user.

---

## Open Issues

### TODO 1 — Deep Research: Vibe Journalism and academic literature

Run the following deep research:

---

**Role and Objective**

Act as a senior academic researcher specialising in Media Studies, Communication Sociology,
and Digital Epistemology. Conduct a multi-step deep research to map the conceptual evolution
of the neologism "Vibe Journalism" — understood as the application of the "Vibe Coding"
paradigm to journalism — and its correspondence within formal scientific literature.

**Research Scope**

In this context, "Vibe Journalism" describes a model of journalistic production based on
semantic intent and natural language communication with AI agents. Instead of following
manual or technical procedural processes (writing code for data journalism, manual source
research), the journalist acts as an orchestrator who provides high-level instructions (the
"vibe") to the AI to perform complex tasks such as report construction, automated
fact-checking, and investigation synthesis.

**Required Analysis Phases**

1. Mapping the Popular Phenomenon (Origin of the Term)
   - Identify the link between the origin of the term "Vibe Coding" (Andrej Karpathy, early 2025)
     and its transposition into journalism (e.g. debates on Substack, X, Nieman Lab).
   - What are the characteristics of "Vibe Journalism" according to media critics and tech
     innovators? (e.g. shift from "writing" to "prompting", democratisation of editorial
     micro-app development, focus on conversational iteration).

2. Translation into Scientific Literature (Peer-Reviewed)
   - Find the academic equivalents that describe this practice of agentic delegation.
   - Analyse in depth the concept of "Agentic Journalism" and the evolution of AI seen
     as "Journalistic Prosthesis".
   - Explore the link with the epistemology of intent and how the literature defines the
     journalist-as-orchestrator relative to the traditional figure.

3. Intersection with Artificial Intelligence (2025/2026 evolution)
   - Analyse how tools like Cursor, Replit, and agents like Claude Code or OpenAI Pulse
     are transforming newsrooms into "AI-native knowledge engines".
   - How does the shift from producing "articles" to providing "structured data and metadata"
     for agentic systems redefine editorial work?

4. Synthesis and Epistemology
   - What are the implications of "Epistemic Ignorance" if the journalist is no longer able
     to explain how the AI agent produced or verified a piece of news?
   - Analyse the risk of "vibe hallucinations" (narrative coherence at the expense of
     precision) and the challenges to journalistic epistemic authority in a human-machine
     co-creation ecosystem.

**Output Format**

- Structure: clear sections with academic headings
- Rigour: references to specific theories and papers (e.g. Social Epistemology, Media Ecology)

**Status**: to do

---

### TODO 2 — Test on a real case

Run a complete analysis using the SpecJournalism workflow on a real case.
Goal: verify in production whether the pre-phases (SJ-1 → SJ-4) produce a more robust
narrative angle compared to running the execution pipeline directly without pre-phases.
**Status**: to do — depends on TODO 1 (context building) and confirmed data availability.

### TODO 3 — Evaluate integration as a Claude Code skill

Evaluate whether the `/sj.*` commands can be implemented as Claude Code skills
(analogous to the Spec Kit → `.claude/skills/` integration).
**Status**: to evaluate after the real-case test (TODO 2).
