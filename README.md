# word-of-the-day

Single-file data feed for a lock screen widget. A Claude Code cloud routine
opens a daily PR updating `word-of-the-day.json`; `.github/workflows/automerge.yml`
merges it automatically. A laptop timer fetches the raw file from `main`.
