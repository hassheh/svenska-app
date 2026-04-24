# Svenska — Vocabulary Practice

A local, single-file spaced-repetition flashcard app for learning Swedish vocabulary. Built for Haseeb based on his Swedish class notes (A1-A2 level).

## How to run it

Because the app loads `vocabulary.json` via `fetch`, you need to serve the folder over HTTP — opening `index.html` directly with `file://` will hit CORS errors. Pick whichever is easiest:

**Python (usually already installed):**
```bash
cd swedish-app
python -m http.server 8000
```
Then open http://localhost:8000

**Node.js:**
```bash
npx serve swedish-app
```

**VS Code:** Install the "Live Server" extension, right-click `index.html` → "Open with Live Server."

## How the spaced repetition works

Simplified Leitner system — each card sits in a "box" (0–5):

| Box | Next review after correct answer |
|-----|----------------------------------|
| 0   | Immediately (new or just missed) |
| 1   | 10 minutes                       |
| 2   | 1 day                            |
| 3   | 3 days                           |
| 4   | 7 days                           |
| 5   | 30 days                          |

**Mastered** = 3 correct in a row → card drops out of rotation.
**Missed** = box drops to `floor(current / 2)`, streak resets to 0, back to front of queue.

## Controls

- **Tap the card** or press **Space** → reveal English
- **Swipe right** or press **→** or click "Knew it" → mark correct
- **Swipe left** or press **←** or click "Didn't know" → mark missed

## Adding vocabulary

Two options:

**1. Edit `vocabulary.json` directly** (recommended for permanent additions):

```json
{
  "swedish": "en katt",
  "english": "a cat",
  "category": "animals",
  "lesson": "2026-04-22"
}
```

Categories used so far: `greetings`, `pronouns`, `questions`, `family`, `work`, `freetime`, `food`, `objects`, `verbs-ar`, `verbs-er`, `verbs-r`, `modal-verbs`, `adjectives`, `countries`, `numbers`, `common`, `connectors`, `school`, `people`, `time`. Feel free to add new ones — the filter chips populate dynamically.

**2. Use the "+ Add words" button** (session-only, won't persist across reloads unless you also update `vocabulary.json`).

**Tip:** When you get new class notes, paste them into Claude and ask: *"Extract new vocabulary from this and append to my vocabulary.json in the same format."* The schema is stable so this works well.

## Progress persistence

Progress is saved to `localStorage` under the key `svenska-vocab-progress-v1`.

- **Export progress** → downloads a JSON backup
- **Import progress** → restore from backup (useful across browsers/devices)
- **Reset progress** → wipes everything, cards go back to "new"

## Architecture notes

Single file, no build step, no dependencies:
- `index.html` → all UI, styles, and logic (vanilla JS)
- `vocabulary.json` → the word list (editable)
- `localStorage` → per-user progress

If you ever outgrow this, the natural upgrade path is:
1. Move progress to IndexedDB for larger history/stats
2. Replace Leitner with proper SM-2 (Anki's algorithm) if you want finer control
3. Add audio pronunciation (Web Speech API can do this with `speechSynthesis` + `sv-SE` voice — might be a fun addition)
4. Add example sentences per card (leveraging what you're doing now in our conversations)

## What's in the starter dataset

~230 cards extracted from your class notes (Apr 13–22, 2026), organized by topic. A focus on A1 survival vocabulary — greetings, family, work, free time, question words, core verbs across all three conjugation patterns (AR/ER/R), modal verbs, adjectives, and the gender articles (`en`/`ett`).

Lycka till! 🇸🇪
