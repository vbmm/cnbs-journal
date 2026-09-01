# CNBS Journal

Local, fully-offline voice trade journal for macOS (Apple Silicon).

Speak or type a trade note → on-device speech-to-text (Parakeet) + on-device LLM (Qwen3.5) structure it into a clean entry → confirm on screen → saved to a local SQLite database. No cloud, no API keys, no telemetry. Your trades never leave your machine.

## Features

- **Voice-first journaling** — dictation ~30 s per trade, text box fallback on the same screen
- **Jargon dictionary** — your shorthand ("GTP", "NAPSO", "STD V2") gets expanded before structuring; fully editable
- **Structured entries** — setup, direction, entry/stop/exit, R multiple, leak tag, confidence, confluences, lesson (≤140 chars)
- **Day logs** — pre-session (state, max loss, game plan) + post-session (grade, 2-loss rule, leaks, passes, lesson)
- **Leak tags** — fomo, revenge, authority, moved_sl, zero_confluence, chased, followed_rules (a clean, rule-following loss is not a leak)
- **Day grades A/B/C/F** — process over PnL: an F with a win is a bad day; an A with a loss is a good day
- **Accounts** — prop-firm style: balance, $/R, daily-loss + max-drawdown limits, live rule gauges, equity curve
- **Guided weekly review** — stats + patterns computed from your own data (leak clusters, state-on-red/green, grade-vs-PnL conflicts), one thing to change
- **Auto-updates** — the app checks GitHub Releases on every launch and updates itself; a failed update never breaks the installed version

## Install (macOS 14+, Apple Silicon)

1. Download `CNBS-Journal-<version>-macOS.zip` from the [latest release](https://github.com/vbmm/cnbs-journal/releases/latest)
2. Unzip → drag **CNBS Journal.app** into **Applications**
3. Open it (first launch: right-click → Open, to pass Gatekeeper on unsigned apps)
4. Wait ~1–2 min on first run while the Python environment builds itself, then ~30 s while models load. The window appears on its own.

### First launch checklist

- **Microphone**: macOS will ask for mic permission on first recording — allow it
- **Models**: the app downloads two open models on first run (Parakeet STT ~2.5 GB, Qwen3.5-4B ~2.7 GB) into the standard HuggingFace cache, unauthenticated. Disk needed: ~6 GB free.
- **Data**: everything lives in `~/.cnbs-journal/` (SQLite DB, config, logs). Delete that folder to fully reset.

## Using it

- **Record tab**: hit the mic (or type), speak your note in plain language — "absorption long NQ, entrée 21 850, stop 21 800, GTP 21 920, plus 2R, focus" — the AI fills the fields, you confirm/fix, save. Every fix teaches it (correction pairs are stored).
- **Journal tab**: your day at a glance — pre-session card (fill it in 30 s before the open), trades, day log, grades.
- **Accounts tab**: create your prop-firm account once; the gauges track drawdown/daily-loss/target in real time.
- **Review tab**: end of week, 10 minutes. The app asks the questions; your data answers them.

## The journaling method

The app implements a process-first journaling course: log every trade, grade the day before looking at PnL, tag every loss with a leak, stop after 2 losses, log passes too (the trades you didn't take), and do a weekly review. A "followed_rules" loss is a clean loss — the cost of the edge, not a leak.

## Updating

Updates are automatic: on launch the app compares its version against the latest GitHub release and self-updates (~30 s). If an update fails to download or verify, the current version keeps running.

## Privacy

- No cloud calls, ever. STT and LLM run on-device (MLX, Metal).
- The only network traffic is the update check against this GitHub repo.
- Your journal DB is local-only and never included in any release package.

## Build / develop

- `app/` — Tauri shell + Next.js UI (`pnpm tauri dev`)
- `journal_core/` — Python sidecar (FastAPI, STT, LLM structuring, SQLite)
- `publish-release.sh` — build + tag + publish a release
- `launcher.sh` — the .app's entry script (update check + service spawn)

## License

MIT