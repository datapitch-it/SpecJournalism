# SpecJournalism — Project Status

## What it is

SpecJournalism is a Spec Driven Data Journalism framework that applies SDD (Spec Driven Development)
principles to the production of AI-orchestrated data journalism analyses.

It wraps around the JournAI technical pipeline (`journoai.md`) by adding pre-phases
(Story Brief, Clarify, Null Hypothesis, Data Design) and cross-artifact consistency gates
(Cross-Check) that run before and during the JournAI execution pipeline.

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

Depends on: `journoai.md` (technical execution pipeline, Phases 1–7), `opensdmx` CLI.

---

## Open Issues

### TODO 1 — Deep Research: Vibe Journalism e letteratura accademica

Eseguire la seguente deep research:

---

**Ruolo ed Obiettivo**

Agisci come un ricercatore accademico senior specializzato in Media Studies, Sociologia della
Comunicazione ed Epistemologia Digitale. Conduci una Deep Research (ricerca approfondita e
multi-step) per mappare l'evoluzione concettuale del neologismo "Vibe Journalism", inteso come
l'applicazione del paradigma del "Vibe Coding" all'ambito dell'informazione, e la sua
corrispondenza all'interno della letteratura scientifica formale.

**Ambito della Ricerca**

In questo contesto, il "Vibe Journalism" descrive un modello di produzione giornalistica
basato sull'intento semantico e sulla comunicazione in linguaggio naturale con agenti IA.
Invece di seguire processi procedurali manuali o tecnici (scrittura di codice per data
journalism, ricerca manuale tra le fonti), il giornalista agisce come un orchestratore che
fornisce istruzioni di alto livello (il "vibe") all'IA per eseguire compiti complessi come
la costruzione di report, il fact-checking automatizzato e la sintesi di inchieste.

**Fasi di Analisi Richieste**

1. Mappatura del Fenomeno Popolare (Origine del Termine)
   - Identifica il legame tra l'origine del termine "Vibe Coding" (Andrej Karpathy, inizio 2025)
     e la sua trasposizione nel giornalismo (es. dibattiti su Substack, X, Nieman Lab).
   - Quali sono le caratteristiche del "Vibe Journalism" secondo i critici dei media e gli
     innovatori tecnologici? (Es. passaggio dal "writing" al "prompting", democratizzazione
     dello sviluppo di micro-app editoriali, focus sull'iterazione conversazionale).

2. Traduzione nella Letteratura Scientifica (Peer-Reviewed)
   - Trova i concetti accademici equivalenti che descrivono questa pratica di delega agentica.
   - Analizza in profondità il concetto di "Agentic Journalism" e l'evoluzione dell'IA vista
     come "Journalistic Prosthesis" (Protesi Giornalistica).
   - Esplora il legame con l'epistemologia dell'intento e come la letteratura definisce la
     figura del giornalista-orchestratore rispetto alla figura tradizionale.

3. Incrocio con l'Intelligenza Artificiale (L'evoluzione nel 2025/2026)
   - Analizza come l'uso di strumenti come Cursor, Replit e agenti come Claude Code o OpenAI
     Pulse stia trasformando le redazioni in "AI-native knowledge engines".
   - In che modo il passaggio dalla produzione di "articoli" alla fornitura di "dati strutturati
     e metadati" per sistemi agentici ridefinisce il lavoro redazionale?

4. Sintesi ed Epistemologia
   - Quali sono le implicazioni della "Epistemic Ignorance" (Ignoranza Epistemica) se il
     giornalista non è più in grado di spiegare come l'agente IA ha prodotto o verificato
     una notizia?
   - Analizza il rischio di "allucinazioni di vibe" (coerenza narrativa a scapito della
     precisione) e le sfide per l'autorità epistemica del giornalista in un ecosistema di
     co-creazione uomo-macchina.

**Formato dell'Output**

- Struttura: sezioni chiare con titoli accademici
- Rigore: riferimenti a teorie e paper specifici (es. Social Epistemology, Media Ecology)
- Lingua: Italiano

**Status**: da eseguire

---

### TODO 2 — Test su caso reale

Eseguire un'analisi completa con il workflow SpecJournalism su uno dei report della
wishlist in `journoai.md` (candidato: Priorità 1 — Mix energetico italiano e dipendenza dal gas).
Obiettivo: verificare in produzione se le pre-fasi (SJ-1 → SJ-4) producono un angolo
narrativo più robusto rispetto al flusso diretto `journoai.md`.
**Status**: da eseguire — dipende da TODO 1 (context building) e da disponibilità dati verificata.

### TODO 3 — Valutare integrazione come Claude Code skill

Valutare se i comandi `/sj.*` possono essere implementati come skill per Claude Code
(analogamente all'integrazione Spec Kit → `.claude/skills/`).
**Status**: da valutare dopo il test su caso reale (TODO 2).
