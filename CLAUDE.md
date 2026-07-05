# Word of the day — routine instructions

This repo backs a lock screen widget. Each day a cloud routine opens a PR that
updates `word-of-the-day.json`; `.github/workflows/automerge.yml` merges it.

## The one rule: never repeat a word

`word-of-the-day.json` carries a `history` array listing **every word ever
used**. The daily word MUST be one that is not already in that array. This is
how repeats are prevented — the file is the routine's memory.

## Authentication: no personal access token

Do **NOT** put a personal access token (PAT) in the routine prompt or in any
file. The Claude Code environment this routine runs in is already authenticated
to GitHub as the repo owner, so no token is needed:

- The **GitHub MCP tools** (`create_branch`, `create_or_update_file`,
  `create_pull_request`, …) act as the authenticated user. Prefer these — they
  need no git checkout and no credentials.
- The checked-out **git remote is pre-authenticated**, so a plain
  `git push origin <branch>` works without any credential helper or token.

If a hardcoded PAT ever appears in the routine, treat it as leaked: revoke it at
https://github.com/settings/tokens and remove it — the routine does not need it.

## Steps for the daily update

1. Read `word-of-the-day.json` and note the `history` array.
2. Pick a new, interesting word whose lowercase form is **not** in `history`.
   If your first pick is already there, choose another until it's genuinely new.
3. Write the file with:
   - `word`, `part_of_speech`, `definition` — for the new word.
   - `date` — today's date in `YYYY-MM-DD` format.
   - `history` — the previous array with the new word appended to the end.
     Never remove or reorder existing entries; only append.
4. Open the PR (via the GitHub MCP tools, or `git push` + `create_pull_request`).
   Only `word-of-the-day.json` may change, or the automerge workflow will
   reject it. Do not touch this file or any other in the daily PR.

## Sanity check before committing

- The new `word` is not present anywhere in the old `history`.
- `history` grew by exactly one entry, and its last element equals the new
  `word`.
- `date` is today and differs from the previous `date`.
