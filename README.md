# The Garage

A single-file lift, cardio and cut tracker: [`the-garage-template.html`](the-garage-template.html),
with no build step and no dependencies.

**Live app:** https://claude.ai/artifact/98UF2bdZkXxmStaSLqVpBZ (private to the owner; bookmark it on every device)

**Read [`GARAGE-HANDOFF.md`](GARAGE-HANDOFF.md) first.** It covers setup, how a new week's
plan reaches the live app, the Sunday review method, and how to republish without wiping
the log. `GARAGESETUP.md` is the original, shorter setup note.

## Train tab

- **Week**: the 7-day plan. Build or edit any day, drop in a saved workout, load a day into today, or roll the plan to next week. In-app edits outrank the seeded `SEED_PLAN`.
- **Workouts**: saved workouts (templates) per place. Create, edit, duplicate, schedule or load into today.
- **Exercises**: about 260 lifts tagged by muscle and equipment, filterable by place, plus your own custom ones.
- **Places**: each location's equipment. An exercise is available at a place when the place has everything it needs.

Places, custom exercises and saved workouts sync in `data/log.json` (`locs`, `lib`, `tpl`) and merge newest-edit-wins per record.

## Weekly plan update (summary)

1. Read the live artifact first, because the log lives there, not in this repo.
2. Replace the `SEED_PLAN` block in *that* source and set `weekOf` to the new week's Monday.
   `SEED_PLAN_U` is derived from `weekOf`, so there's nothing to bump by hand.
3. Publish back to the same URL. Each device adopts the new week on its next load.

Never set `SEED_U` or `SEED_PLAN_U` to `Date.now()`, because that causes an endless republish loop.

## Config

The Log tab takes weight and macros from Apple Health, e.g. MyFitnessPal. Setup: [`HEALTH-SHORTCUT.md`](HEALTH-SHORTCUT.md).

At the top of the main `<script>`: `GYM`, `OWNER`, `T` (daily targets), `START_WEIGHT`,
`TRAVEL_CAL` (higher calorie caps for trips), `SEED_PRS` (lifts from before the app) and `SEED_PLAN`.
