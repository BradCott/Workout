# The Garage

A single-file lift, cardio and cut tracker, rebuilt from the setup guide in
[`GARAGESETUP.md`](GARAGESETUP.md). The whole app is
[`the-garage-template.html`](the-garage-template.html): no build step and no dependencies.

**Live app:** https://claude.ai/artifact/98UF2bdZkXxmStaSLqVpBZ (private to the owner; bookmark it on every device)

## Tabs

- **Today**: 7-day weight average, a 30-day trend chart, today's macros against targets (a bar turns red on a miss), and the week at a glance.
- **Log**: weight, macros, lifts (sets × reps, "last time" hints, 🏆 on new PRs), cardio and notes. Tap ✓ on a set to start the rest timer.
- **Plan**: the weekly program (`SEED_PLAN`). "Load into log" pre-fills a whole session.
- **PRs**: heaviest set and best estimated 1RM (Epley) per lift.
- **History**: every logged day, newest first. Tap one to open it.
- **Export**: a plain-text weekly review to paste into Claude, Sync now, JSON backup and import, and diagnostics.

## Make it yours

Edit the config block at the top of the `<script id="garage-app">`: `GYM`, `OWNER`, `T` (targets),
`START_WEIGHT`, `UNIT` and `SEED_PLAN`.

## How sync works

Your log lives inside the published page as JSON (`<script id="garage-data">`). When you leave
the Log tab or tap Sync, the page republishes itself with the merged data, and every open
copy reloads to the new version. Between syncs, every edit saves to that device's browser
storage. Merging is per day, and the newest edit wins.

> ⚠️ **Before republishing this file after changing the code:** the live artifact holds your
> data and this repo copy does not. Read the live artifact first and carry its
> `garage-data` block into the new version, or export a JSON backup and import it afterwards.
> Otherwise republishing resets the published log. Days still saved on a device merge back in
> the next time you sync from that device.
