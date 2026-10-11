# Bangla Flashcards

[![license MIT](https://img.shields.io/badge/license-MIT-green)](LICENSE)
[![offline single file](https://img.shields.io/badge/offline-single--file%20HTML-blue)](https://bt108.github.io/bangla-flashcards/)

An offline, single-file flashcard app for learning Bangla — Leitner spaced
repetition, English ⇄ Bangla. No build step, no network, no dependencies.
For Bangla learners studying vocabulary, letters, conjuncts, and sentences
with daily reviews.

**Live demo:** https://bt108.github.io/bangla-flashcards/
(served straight from this repo via GitHub Pages)

## 30-second example

Deck text — blank line separates cards, `--` splits front from back:

```text
Hello
--
হ্যালো (hyalo)

Thank you
--
ধন্যবাদ (dhonnobad)
```

```sh
python3 -m http.server 8000
# → http://localhost:8000 — paste the snippet into "Add deck", Save deck,
#   then Study. (Or just open index.html in a browser; no server needed.)
```

Decks and review state persist in the browser (IndexedDB).

## Sample output

Seeded with the 4 builtin decks, rendered in Chromium:

| Desktop | Mobile |
|---|---|
| ![Deck list on desktop](docs/img/decks.png) | ![Deck list on mobile](docs/img/mobile.png) |

## Prerequisites

- A modern browser — runs the app. Deck data and per-card review state use
  IndexedDB; typography prefs use localStorage.
- Python 3 *(optional)* — only for `python3 -m http.server` local hosting;
  opening `index.html` directly works too.
- A Bengali font *(optional)* — card font/size are adjustable in the footer
  bar, including system Bengali fonts via Local Font Access where supported.

## Commands

No build step and no scripts. Two ways to run:

```sh
python3 -m http.server 8000   # serve locally → http://localhost:8000
```

or open `index.html` in any modern browser.

Keyboard: `Space` reveal · `1` Known · `2` Hard · `3` Unknown · `F` focus ·
`Esc` quit.

## Project layout

```
index.html          # the entire app: CSS + UI + SRS core + storage
docs/img/           # decks.png + mobile.png renders for this README
LICENSE             # MIT
README.md           # this file
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

Fronts must be unique within a deck. Any violation rejects the whole import
with a deck + card + line-range error.

## Content model

- **Decks** are plain text in `flashdeck-v1` format; 4 builtin decks ship
  inside `index.html` (starter vocabulary 50, letters 60, conjuncts 81,
  sentences 20).
- **Leitner-5 SRS**: 5 boxes, review intervals 1/3/7/14/30 days. Known →
  box+1, Hard → box−1, Unknown → box 1; at most 20 new cards per day.
- **Study queue** is one pass over all due + all new cards, shuffled by
  default or in deck order.
- **State** (`{box, due}` per card plus the daily-new counter) persists to
  IndexedDB after every grade; JSON backup/restore re-validates on import.
- **Session summary** reports reviewed/known/hard/unknown, accuracy, boxes,
  and 24h-due counts.

## License

MIT — see [LICENSE](LICENSE).
