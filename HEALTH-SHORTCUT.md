# Apple Health → The Garage

A web page can't read Apple Health, so an iPhone Shortcut reads today's numbers and
copies them. You paste them into the Log tab's **From Apple Health** box. That fills in
weight and macros only; lifts, cardio and notes are never touched.

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
4. Repeat steps 2–3 for *Protein* → **Pro**, *Carbohydrates* → **Carb** and
   *Total Fat* → **Fat**.
5. **Date** → *Current Date*, then **Format Date** → *Custom*: `yyyy-MM-dd`.
6. **Text** — type this, inserting the variables where shown:
   ```
   date: [Formatted Date]
   weight: [Weight]
   cal: [Cal]
   pro: [Pro]
   carb: [Carb]
   fat: [Fat]
   ```
7. **Copy to Clipboard** (input: the Text).
8. *(Optional)* **Open URLs** → your app link, so it opens right after copying.

The first run asks permission to read each Health type. Allow them all.

## 3. Daily use

Run **Garage Health** (home-screen icon, widget or "Hey Siri, Garage Health") → open the
app → Log tab → tap the box → **Paste**. It applies itself when you paste.

## What the app accepts

- `key: value` lines in any order. Units and commas are ignored (`198.4 lb`, `1,950`).
- Blank or `0` means "no data" and is skipped, so it never overwrites with zeros.
- Several days at once: each `date:` line starts a new day.
- Without a `date:` line it goes to whichever day the Log tab is showing.
- Keys: `weight`, `cal`/`calories`, `pro`/`protein`, `carb`/`carbs`, `fat`.

Manual entry is still there under **Enter manually** on the Log tab.
