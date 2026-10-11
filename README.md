# Bangla Flashcards

Offline, single-file flashcard app for learning Bangla — Leitner spaced
repetition, English ⇄ Bangla. No build step, no network, no dependencies:
open `index.html` and study.

**Live demo:** `https://bt108.github.io/bangla-flashcards/`
(served straight from this repo via GitHub Pages)

## Features

- **Leitner-5 SRS** — 5 boxes, intervals 1/3/7/14/30 days; Known → box+1,
  Hard → box−1, Unknown → box 1; max 20 new cards introduced per day.
- **Ships with 4 builtin decks** — starter vocabulary (50 words), Bangla
  letters (60 cards), combined forms / conjuncts (81 cards), sentences (20).
- **Deck import** — paste text or drop a `.txt` file. Format: blank line
  separates cards, one `--` line splits front from back, either side may
  span lines, `#` headers allowed. Whole import rejected with a
  deck + card + line-range error on any violation.
- **JSON backup / restore** — per-deck export with versioned format and full
  SRS state; re-validated on import.
- **Persistence** — decks + per-card `{box, due}` state in IndexedDB,
  typography prefs in localStorage. Seeded builtins never resurrect
  user-deleted decks.
- **Study UX** — shuffled or in-order mode, focus mode (`F`), keyboard
  grading (`Space`, `1/2/3`, `Esc`), session summary with accuracy, boxes,
  and 24h-due counts.
- **Typography controls** — card font family/size, including Local Font
  Access enumeration for system Bengali fonts.

## Run

No toolchain. Open `index.html` in any modern browser, or serve it:

```sh
cd bangla-flashcards
python3 -m http.server 8000
# → http://localhost:8000
```

## Project layout

```
index.html   the entire app: CSS + UI + pure SRS core + storage
README.md    this file
LICENSE      MIT
```

The SRS core (`parseDeckText`, `selectQueue`, `gradeState`, …) is DOM-free
and exposed as `window.__flash` for headless testing.

## Deck format (flashdeck-v1)

```text
# optional # comment header (only before card content)

Front text, may span
several lines
--
Back text, may also
span lines

Next front
--
Next back
```

Fronts must be unique within a deck.

## Status

Working offline app, used for real Bangla study. Single file by design —
easy to copy, share, and host anywhere.
