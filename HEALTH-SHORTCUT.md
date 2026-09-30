# Apple Health → The Garage

A web page can't read Apple Health, so an iPhone Shortcut reads today's numbers and
opens the app with them in the link. The app fills them in by itself, with no pasting.
It only fills in weight and macros. Lifts, cardio and notes are never touched.

## 1. Get MyFitnessPal into Apple Health (one time)

- **MyFitnessPal:** Settings → Sharing & Privacy (or "Apps & Devices") → Apple Health →
  allow it to **write** Nutrition (energy, protein, carbohydrates, fat). The menu wording
  varies by MFP version.
- **Weight:** a smart scale that syncs to Health, or type it into Health / MFP.

## 2. Build the Shortcut (about 10 minutes)

Shortcuts app → **+** → name it **Garage Health**. Add these actions in order:

1. **Find Health Samples** → Type: *Weight* · Start Date *is today* ·
   Sort by *Start Date*, *Latest First* · Limit *1*.
   Rename the result **Weight** (tap it → Rename).
2. **Find Health Samples** → Type: *Dietary Energy* · Start Date *is today*.
3. **Calculate Statistics** → *Sum* of the samples from step 2. Rename it **Cal**.
4. Repeat steps 2–3 for *Protein* → **Pro**, *Carbohydrates* → **Carb**,
   *Total Fat* → **Fat** and *Active Energy* → **Burn** (unit *kcal*). Burn is what
   powers the Today card's deficit and workout suggestions.
5. **Date** → *Current Date*, then **Format Date** → *Custom*: `yyyy-MM-dd`.
   Add a second **Format Date** on *Current Date* → *Custom*: `HHmmss`, and rename it **Stamp**.
6. **Text**: type this on one line, inserting the variables (blue bubbles) where shown:
   ```
   https://claude.ai/artifact/98UF2bdZkXxmStaSLqVpBZ?r=[Stamp]#h_[Formatted Date]_w[Weight]_c[Cal]_p[Pro]_cb[Carb]_f[Fat]_ae[Burn]
   ```
   `?r=[Stamp]` makes every run a brand-new link. Without it, when the app is already
   open iOS can treat the link as the page you're on and switch to it without passing
   the numbers along.
7. **Copy to Clipboard** (input: the Text). This is the backup, see below.
8. **Open URLs** (input: the Text).

Keep the link on one line. Anything after a space or line break is cut off, so
check the box under the last action after a run and make sure every number is there.

The first run asks permission to read each Health type. Allow them all.

### Optional: send yesterday's final totals too

The Weekend bank scores each day on the last numbers the Shortcut sent for it. If you
only run it at lunch, that day looks like you barely ate. To fix that automatically,
have every run also send **yesterday's** full totals:

1. Add **Find Health Samples** → Type *Dietary Energy* · Start Date *is yesterday*, then
   **Calculate Statistics** → *Sum*, then **Set Variable** → **YCal**.
2. Same again for *Active Energy* → **YBurn**.
3. Put these six actions above the Text action, and add to the end of the link:
   `_yc[YCal]_yae[YBurn]`

The first run of each day then locks in the day before.

## 3. Daily use

Run **Garage Health** from the home-screen icon, a widget or "Hey Siri, Garage Health".
The app opens on the Log tab with today's numbers filled in. Running it again later in
the day updates them.

To make it automatic: Shortcuts → **Automation** → **+** → *Time of Day* (e.g. 9 pm) →
*Run Immediately* → Garage Health. It still opens the app. iOS won't let a web page
sync silently in the background.

## If the app opens but nothing fills in

Sometimes iOS opens the app but drops the numbers from the end of the link. Because
step 7 copies the link too, go to **Log** → **From Apple Health**, tap the box and paste.
The numbers fill in straight away, the same as if the link had worked.

## Pasting instead

The **From Apple Health** box still works if you prefer copying: swap step 6 for
`key: value` lines (`date:`, `weight:`, `cal:`, `pro:`, `carb:`, `fat:`) and step 7 for
**Copy to Clipboard**.

## What the app accepts

- `key: value` lines in any order. Units and commas are ignored (`198.4 lb`, `1,950`).
- Blank or `0` means "no data" and is skipped, so it never overwrites with zeros.
- Several days at once: each `date:` line starts a new day.
- Without a `date:` line it goes to whichever day the Log tab is showing.
- Keys: `weight`, `cal`/`calories`, `pro`/`protein`, `carb`/`carbs`, `fat`, `active` (burned), `resting`.
- Link codes: `w` weight, `c` cal, `p` protein, `cb` carbs, `f` fat, `ae` active energy, `re` resting energy.
  `yc` / `yae` are yesterday's food and active energy.

Manual entry is still there under **Enter manually** on the Log tab.
