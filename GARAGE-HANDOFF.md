# The Garage — handoff

Everything another Claude needs to run this app for a new lifter. Ships with
`the-garage-template.html`: the current version of Cole's app with his data,
branding and plan removed. Nothing here needs patching first.

---

## 1. Setup (about two minutes)

Put `the-garage-template.html` somewhere Claude Code can read it — a home or
projects folder. **Avoid Downloads, Desktop and Documents**; macOS blocks apps
from reading those until permission is granted, and it will simply fail to open.

Then ask Claude Code:

> Publish the-garage-template.html as an artifact with the `artifact` and
> `downloads` capabilities. Set the favicon to 🏋️.

You get back a private URL — that's the app. Open that same link on every device
(phone, laptop), because the shared link is what makes sync work. On iPhone, open
it in Safari and use Share → Add to Home Screen.

**Only ever use the published link.** Opening the `.html` file from disk also
works and also saves, but only into that one browser, invisible to everything
else. The sync chip under the title will say *This device only*.

Edit the config block near the top of the `<script>` to make it yours:

```js
const GYM   = 'THE GARAGE';   // big title
const OWNER = 'YOUR GYM';     // small line above it
const T = { cal:2000, proMin:170, proMax:200, fatMax:70, carb:200 };
const START_WEIGHT = 200;
```

The Log tab's targets line is generated from `T`, so it can never disagree with
the numbers above.

---

## 2. How a new week gets into the live app

**The plan is not the code.** `SEED_PLAN` is a seed. Once the app has run, the
live plan lives in the synced state (`DB.plan`), which sits in localStorage on
each device and in the artifact's `data/log.json`. On load, the device's copy and
the cloud copy merge, and for the plan whichever side has the newer `planU` wins.
That is why editing `SEED_PLAN` alone appears to do nothing.

**What makes the edit take effect:**

```js
const SEED_PLAN_U = Date.parse(SEED_PLAN.weekOf+'T00:00:00Z');
```

and in `load()`:

```js
if(!DB.plan || (DB.planU||0)<SEED_PLAN_U){ DB.plan=SEED_PLAN; DB.planU=SEED_PLAN_U; }
```

The stamp is derived from the plan's own `weekOf`, so a new week automatically
outranks the synced one. **There is nothing to bump by hand.** Just make sure
`weekOf` is the new week's Monday and that it is later than the previous week's.

So the weekly routine is exactly: replace the `SEED_PLAN` block, set `weekOf`,
republish the page. Each device adopts the new week on its next load and passes
it along.

**Never set `SEED_PLAN_U` or `SEED_U` to `Date.now()`.** A stamp that changes on
every load makes seeded data look freshly edited, which triggers a publish, which
reloads the page, which stamps again — an endless republish loop.

### Republishing without destroying the log

State lives in two places: `data/log.json`, and an embedded
`<script id="seed">` block inside the page. Publishing a page replaces that seed
block. **If you publish a stale local copy, you can wipe the log.**

The safe sequence every time:

1. Read the live artifact (`Artifact` tool, `action:"read"` with the URL).
2. Apply the plan edit to *that* source.
3. Publish the merged result back to the same URL.

Never publish a local file that hasn't been reconciled with what's live. The
Export tab's Diagnostics shows a `form used` line telling you which storage path
this install is on.

### Plan data shape

```js
const sets = (n,w,r) => Array.from({length:n},()=>({w:String(w),r:r==null?'':String(r),done:false}));

const SEED_PLAN = {
  weekOf:'2026-09-28',                       // that week's Monday
  days:{
    '2026-09-28':{kind:'lift', title:'Push', sub:'5 exercises · 16 sets', ex:[
      {name:'Barbell Bench Press', rx:'4×6 @ 135', rest:150, note:'Coaching cue.', sets:sets(4,135,6)},
      {name:'Wide-Grip Pull-Ups',  rx:'4×max',     rest:90,  note:'',             sets:sets(4,'BW')},
      {name:'Lateral Raise',       rx:'3×15 @ 15', rest:60,  note:'Finisher.', opt:true, sets:sets(3,15,15)},
    ]},
    '2026-09-30':{kind:'rest', title:'Rest', sub:'Walk if you feel like it', ex:[]},
  }
};
```

- `kind`: `lift` and `flex` get a "Load into today" button; `rest`, `hiit` and
  `travel` don't. `flex` renders a "Droppable" badge, `travel` a "Travel" badge.
- `rest` is seconds and drives the rest timer. `rx` is the target label shown in
  gold. `opt:true` adds an "Optional" badge.
- Weights are strings, so `'BW'` and `'band'` are valid.
- Prefilled sets are flagged as targets. They do **not** count as work done, do
  not create PRs and do not mark the day trained until a set is ticked or typed
  into.
- Dates should be the week ahead, but "Load into today" always writes into
  today's log regardless, so a stale plan can't overwrite an old day.

### Other things that live in the code, not the data

- `T` — daily macro targets.
- `TRAVEL_CAL` — a list of `{from, to, max}` windows where a higher calorie cap
  counts as on-target. Past trips stay in the list so old days keep being judged
  against the cap that applied then.
- `SEED_PRS` — lifts whose numbers predate the app.
- `LOST_SESSION_PRS` — PRs whose session no longer exists in the log. Both act as
  a floor under the PR board, which is otherwise recomputed from the log on every
  save, so fixing a typo or deleting a day corrects the board.

---

## 3. The Sunday review

There is no saved prompt, skill or CLAUDE.md behind this. It is an ordinary chat:
the Export tab's weekly summary gets pasted in, and next week comes back. What
persists between weeks is a **Rules sheet** carried in the weekly spreadsheet and
re-uploaded each Sunday. If you want that continuity, start one in week one and
ask for it back with every weekly paste. Otherwise every Sunday starts from zero.

**The input.** The Export tab produces: the week and days logged, the weight
series, nutrition averages with a cal+protein hit count, cardio totals, plan
adherence, every lift with its sets and a 🏆 on PR days, then the day notes.

**The output.** A spreadsheet with three sheets. *Week Plan* (columns: Date, Day,
Location, Session, #, Exercise, Equipment, Sets, Reps, Weight, Rest, Coaching
Note), *Nutrition* (targets, travel adjustments, current state), and *Rules* (the
accumulated rules plus a current PR board).

### The method

- Add weight only after hitting the **target reps on every set** with clean form.
  One good set is not enough. A lift that went 7,7,6,6 holds where it is.
- Dropping weight to fix form counts as progress. Progress by reps until the
  pattern is clean, then add load.
- 5–8 movements per session, 18–24 working sets. Never more than 8 movements.
- Heaviest and most technical lift first, while fresh. After that, order by
  equipment — with one barbell, re-racking costs more than resequencing.
- Don't add an extra session on top of a completed week. Extra fatigue in a
  deficit costs more than the volume gains.
- Bodyweight movements progress by logged reps, so the reps have to get logged.

### Calories, from the trend and not from any single weigh-in

- Judge the 7-day average, never a daily reading.
- Losing faster than ~2 lb/week starts costing muscle rather than fat: eat more.
- Flat for two weeks running means that intake is maintenance: drop about 100.
- Dropping 1–1.5 lb/week: hold.
- The dashboard encodes the same thresholds: 1–1.75 lb/week shows green, above 2
  shows red with "too fast — add 150 cal", below 0.75 says "slow — hold steady".
- Protein is the floor that doesn't move. Travel days eat higher on purpose;
  under-eating on the road is the worse outcome.

**These numbers are Cole's.** His targets, body weight and every listed load come
from his own history. What transfers is the method, not the numbers. Set fresh
starting numbers from this lifter's own first two or three weeks of logs.

---

## 4. Things worth knowing

- **Saving has two speeds.** Typing in the Log tab saves to the device instantly.
  Leaving the Log tab, or tapping Sync now, pushes to the other devices. That
  split exists because syncing refreshes the page, so it waits until you're done
  logging. The open tab, the day being edited and a running rest timer all
  survive that refresh.
- **Data merges rather than overwrites** — newest edit per day wins — so logging
  on a phone and a laptop on the same day won't clobber either one.
- **Back up now and then.** Export → Download JSON. Some browsers block storage,
  and when they do the published page is the only copy. Import JSON merges a
  backup back in without duplicating anything.
- **The rest timer** counts against the clock rather than ticking down, so
  backgrounding the app can't make it drift. On iPhone the beep needs one tap
  anywhere in the app first; tapping ✓ counts.
- **Troubleshooting:** Export tab → Diagnostics reports whether the runtime
  connected, whether storage works, and the exact error from the last publish.
  Paste that rather than describing the symptom.
