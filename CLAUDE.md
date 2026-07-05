# Word of the day — routine instructions

This repo backs a lock screen widget. Each day a cloud routine opens a PR that
updates `word-of-the-day.json`; `.github/workflows/automerge.yml` merges it.

## The one rule: never repeat a word

`word-of-the-day.json` carries a `history` array listing **every word ever
used**. The daily word MUST be one that is not already in that array. This is
how repeats are prevented — the file is the routine's memory.

## Steps for the daily update

1. Read `word-of-the-day.json` and note the `history` array.
2. Pick a new, interesting word whose lowercase form is **not** in `history`.
   If your first pick is already there, choose another until it's genuinely new.
3. Write the file with:
   - `word`, `part_of_speech`, `definition` — for the new word.
   - `date` — today's date in `YYYY-MM-DD` format.
   - `history` — the previous array with the new word appended to the end.
     Never remove or reorder existing entries; only append.
4. Open the PR. Only `word-of-the-day.json` may change, or the automerge
   workflow will reject it.

## Sanity check before committing

- The new `word` is not present anywhere in the old `history`.
- `history` grew by exactly one entry, and its last element equals the new
  `word`.
- `date` is today and differs from the previous `date`.
