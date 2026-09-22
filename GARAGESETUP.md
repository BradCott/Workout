# The Garage — workout tracker

A single-file lift, cardio and cut tracker. Log sets with a rest timer that runs
itself, track weight against a 7-day average, and see the week at a glance. Your
data syncs across every device you open it on.

Everything is in `the-garage-template.html`. No build step, no dependencies, no
accounts to create.

---

## Setup (about two minutes)

Put `the-garage-template.html` somewhere Claude Code can read it — your home
folder or a projects folder is fine. **Avoid Downloads, Desktop and Documents**;
macOS blocks apps from reading those until you grant permission, and it'll just
fail to open the file.

Then tell Claude Code:

> Publish the-garage-template.html as an artifact with the `artifact` and
> `downloads` capabilities. Set the favicon to 🏋️.

You'll get back a private URL — that's your app. Bookmark it on your phone and
your laptop. Same link on both; that's what makes the sync work.

**On iPhone:** open the link in Safari, then Share → Add to Home Screen for a
one-tap icon.

---

## Make it yours

Near the top of the `<script>` block there's a short config block. Edit it
directly, or ask Claude Code to:

```js
const GYM   = 'THE GARAGE';   // big title
const OWNER = 'YOUR GYM';     // small line above it
const T = { cal:2200, proMin:180, proMax:210, fatMax:80, carb:200 };
const START_WEIGHT = 200;     // weight you started at
```

`T` is your daily targets — calories, protein range, fat ceiling, carbs. The
dashboard colours your macro bars red when you miss them, so set these to what
you're actually chasing.

The **Plan** tab ships with a generic push/pull/legs week as a worked example.
Replace it with your own program — hand Claude Code your routine and ask it to
rewrite `SEED_PLAN`. Each day holds exercises with target weights, rest times
and notes; "Load into log" then pre-fills a whole session so you just tick sets
off as you go.

The PR board and history start empty and fill in as you log.

---

## How saving works

Two speeds, and it matters that you know which one you're in:

- **Typing in the Log tab** saves to your device instantly. No interruption.
- **Leaving the Log tab, or tapping Sync now** pushes to your other devices.

That split is deliberate. Syncing refreshes the page, so it waits until you're
done logging rather than doing it mid-set. Your open tab, the day you're
editing, and a running rest timer all survive that refresh.

The chip under the title tells you where you stand: *Synced*, *Saved here — tap
to sync*, or *This device only*.

Data merges rather than overwrites — newest edit per day wins — so logging on
your phone and your laptop on the same day won't clobber either one.

---

## Two things that will bite you

**Only ever use the published link.** If you open the `.html` file directly from
your hard drive, it still works and still saves — but only to that one browser,
invisible to everything else. The chip will say *This device only*. Delete your
local copy once you've published, so there's nothing to open by accident.

**Back it up now and then.** Export → Download JSON. Some browsers block storage
in this context, and when they do, the published page is the only copy of your
log. Import JSON merges a backup back in without duplicating anything.

---

## Rest timer

Tap the ✓ on a set and the timer starts at that exercise's rest time. It counts
against the clock rather than ticking down a number, so backgrounding the app or
locking your phone can't make it drift — you come back to the true time
remaining. It beeps and buzzes at zero, holds the screen awake while it runs,
and `+15` / `−15` adjust on the fly.

On iPhone the beep needs one tap anywhere in the app first — iOS won't let a
page play audio until you've interacted with it. Tapping the ✓ counts, so in
practice it just works.

---

## Weekly review

The Export tab builds a plain-text summary of the week — weights, macro
averages, cardio, every lift with a 🏆 on new PRs, and your notes. Copy it into
a chat with Claude and ask for next week's numbers and a new plan. That's the
loop this was built around.

---

## Troubleshooting

**Chip says "This device only"** — you're on a local file rather than the
published link, or the page wasn't published with the `artifact` capability.

**Something looks wrong with syncing** — Export tab, bottom, Diagnostics. It
reports whether the runtime connected, whether storage works, and the exact
error from the last publish. Paste that to Claude Code rather than describing
the symptom; it names the actual cause.

**Changes not showing on your other device** — open the link fresh there. A tab
that's been sitting open can be on a cached older copy; hard-reload it
(Cmd+Shift+R), or fully close and reopen the home-screen app on iPhone.
