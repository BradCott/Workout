# The Garage

A single-file lift, cardio and cut tracker: [`the-garage-template.html`](the-garage-template.html),
with no build step and no dependencies.

**Live app:** https://claude.ai/artifact/98UF2bdZkXxmStaSLqVpBZ (private to the owner; bookmark it on every device)

**Read [`GARAGE-HANDOFF.md`](GARAGE-HANDOFF.md) first.** It covers setup, how a new week's
plan reaches the live app, the Sunday review method, and how to republish without wiping
the log. `GARAGESETUP.md` is the original, shorter setup note.

## Weekly plan update (summary)

1. Read the live artifact first, because the log lives there, not in this repo.
2. Replace the `SEED_PLAN` block in *that* source and set `weekOf` to the new week's Monday.
   `SEED_PLAN_U` is derived from `weekOf`, so there's nothing to bump by hand.
3. Publish back to the same URL. Each device adopts the new week on its next load.

Never set `SEED_U` or `SEED_PLAN_U` to `Date.now()`, because that causes an endless republish loop.

## Config

At the top of the main `<script>`: `GYM`, `OWNER`, `T` (daily targets), `START_WEIGHT`,
`TRAVEL_CAL` (higher calorie caps for trips), `SEED_PRS` (lifts from before the app) and `SEED_PLAN`.
