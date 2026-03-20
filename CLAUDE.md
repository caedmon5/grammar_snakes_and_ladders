# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

"Prove It!" is a mountain-climbing grammar game — a single-page HTML application used in classroom settings. One group at a time attempts to "summit" by completing 5 syntax analysis stages. The competitive element is completing in the fewest turns. Companion game cards are provided as `.docx` and `.pdf` files.

## Running

Open `index.html` directly in a browser. No build step, package manager, or dependencies exist. To serve locally:

```
python -m http.server 8000
# then visit http://localhost:8000
```

## Architecture

The entire app lives in `index.html` (HTML + embedded CSS + embedded JS). There is no build system, no framework, and no tests.

**Layout**: Split panel — left column (35%) shows an inline SVG mountain with climbing path, camp markers, and climber figures; right column (65%) shows the active stage interaction panel.

**Game flow (5 stages / camps)**:
1. Identify Phrase Type — Correct/Incorrect buttons
2. Prove Phrase Type — Random test assignment with Proven/Try Another/Incorrect
3. Identify Syntactic Function — Correct/Incorrect buttons
4. Prove Syntactic Function — Same random-test mechanics as stage 2
5. Draw Syntax Tree — Bracket notation textarea with real-time canvas tree rendering

**State**: `state` object with `groups[]` (each has `stageIndex`, `turns`, `currentTestIdx`, `testsUsed[]`, `bracketInput`, `completed`), `activeGroupIndex`, `view`. Persisted to `localStorage` under key `"proveItMountain"`.

**Rendering**: `render()` calls `renderTabs()`, `renderMountain()`, `renderStage()`. `save()` is called at the end of every render.

**Constants**: `STAGES` (5 stage definitions), `PHRASE_TYPES` (NP/VP/AdjP/AdvP/PP), `FUNCTIONS` (10 syntactic functions), `TESTS` (9 tests with name/desc/example).

**Tree rendering**: Recursive descent bracket parser (`parseBrackets`) + Canvas-based tree renderer (`drawTree`). Leaves get sequential x-positions, internal nodes are centered over children.
