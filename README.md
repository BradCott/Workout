# The Garage

A single-file lift, cardio and cut tracker: [`the-garage-template.html`](the-garage-template.html),
with no build step and no dependencies.

**Live app:** https://claude.ai/artifact/98UF2bdZkXxmStaSLqVpBZ (private to the owner; bookmark it on every device)

**Read [`GARAGE-HANDOFF.md`](GARAGE-HANDOFF.md) first.** It covers setup, how a new week's
plan reaches the live app, the Sunday review method, and how to republish without wiping
the log. `GARAGESETUP.md` is the original, shorter setup note.

## Today card (Home)

Energy balance for the day, from the Health Shortcut:
- **Burned:** resting burn for the whole day plus Apple Health active energy at 85%, projected to `BEDTIME`.
  Resting burn uses `ME` (age, height, sex) with Mifflin-St Jeor, or ~8.2 cal per lb until those are set.
  The watch comes off at night, so resting burn is estimated rather than read from Health.
- **Target:** `GOAL_LBWK` × 3,500 / 7 cal under the burn (2 lb/week = 1,000/day).
- **Calibration:** after 14+ days of food and burn, plus 8+ weigh-ins over 2+ weeks, burn is scaled
  (0.8–1.2) so the estimated deficit matches the actual weight trend.
- **Coach:** today's planned lift, or a saved workout that hits muscles not trained in 3 days.
  If you're over target, it gives cardio options sized to the gap (max 60 min each).
  Those use your own Peloton output and walk pace once logged.

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

At the top of the main `<script>`: `GYM`, `OWNER`, `T` (daily targets), `START_WEIGHT`, `ME`, `GOAL_LBWK`, `BEDTIME`,
`TRAVEL_CAL` (higher calorie caps for trips), `SEED_PRS` (lifts from before the app) and `SEED_PLAN`.

## Open items

- Add yesterday's totals to the Health Shortcut (`_yc`, `_yae`; see HEALTH-SHORTCUT.md) so the weekend bank sees full days.
- Mon 9/21 and Tue 9/22 are hand-entered as 800-cal deficits (`BANK_SEED`): confirm whether that meant 800 under burn (−200 vs target each) or 800 better than target.
- Gymverse: real weight of the 5/14 cable row 4th set ("1145 lb"), and the date of the first incline DB session.
