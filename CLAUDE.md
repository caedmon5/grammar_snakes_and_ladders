# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

"Prove It!" is a grammar game scoreboard — a single-page HTML application used in classroom settings to track team progress. Companion game cards are provided as `.docx` and `.pdf` files.

## Running

Open `prove_it_scoreboard.html` directly in a browser. No build step, package manager, or dependencies exist. To serve locally:

```
python -m http.server 8000
# then visit http://localhost:8000/prove_it_scoreboard.html
```

## Architecture

The entire app lives in `prove_it_scoreboard.html` (HTML + embedded CSS + embedded JS, ~230 lines). There is no build system, no framework, and no tests.

**State**: A `teams` array (4 groups, each with `name`, `color`, `pos`) and a `round` counter. Persisted to `localStorage` under key `"proveIt"`.

**Rendering**: `render()` rebuilds all lane DOM each call. `save()` is called at the end of every render. `move(idx, delta)` clamps score to `[0, MAX]` (MAX = 15) then re-renders.

**Scoring buttons**: +1 (gate cleared), +2 (self-correction bonus), -1 (undo), +3 (bonus). A winner banner appears when any team reaches MAX.
