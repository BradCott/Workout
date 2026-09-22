# The Garage

A single-file lift, cardio and cut tracker. The whole app is
[`the-garage-template.html`](the-garage-template.html), with no build step and no dependencies.
Setup notes are in [`GARAGESETUP.md`](GARAGESETUP.md).

**Live app:** https://claude.ai/artifact/98UF2bdZkXxmStaSLqVpBZ (private to the owner; bookmark it on every device)

## What's in it

- **Home**: this week at a glance (ride, walk and lift dots plus a macro hit/miss bar), a day streak, weight, the 7-day average, lb/week loss rate with coaching, 7-day nutrition averages, Peloton output and walk-mile charts, and the PR board.
- **Plan**: the weekly program (`SEED_PLAN`). "Load into log" pre-fills the session with target weights and rest times.
- **Log**: weight, MyFitnessPal (MFP) totals, Peloton, walk and HIIT toggles, and lifts. Tap ✓ on a set and the rest timer runs itself.
- **History**: every logged day. Tap one to edit it.
- **Export**: a weekly summary to paste into Claude, Sync now, backup and restore, diagnostics, and clear all.

## Make it yours

Edit the config block at the top of the main `<script>`: `GYM`, `OWNER`, `T` (daily targets),
`START_WEIGHT` and `SEED_PLAN`. `EX_SUGGEST` holds the exercise-name autocomplete list.

## How sync works

It saves to this device's browser storage instantly. When sync runs, it publishes the log as
`data/log.json` inside the artifact, which reaches every device without a page reload. If
that isn't allowed, it falls back to republishing the whole page with the log embedded in
`<script id="seed">`. Merging is per day, and the newest edit wins.

> ⚠️ Your log lives in the published artifact, not in this repo. Republishing the HTML keeps
> `data/log.json`, but if sync ever used the whole-page fallback, carry the live page's `seed`
> block forward (or export a backup first) before republishing a code change.

## Changes from the original template

- The Plan tab maps a plan dated to a past week onto the current week, so "Load into log" logs to today rather than the plan's original date.
- The Log tab's targets hint now reads from `T` instead of hardcoded numbers.
