# Gym Tracker

A personal workout logger that runs entirely in your phone's browser. No account, no server, no build step: one `index.html`.

- **Works offline**: a service worker caches the app, so it opens at the gym with no signal.
- **Installable**: open the site on your phone → Share / ⋮ menu → *Add to Home Screen*.
- **Private**: data lives in the browser's `localStorage` on your device. Use **Settings → Copy backup** now and then.

## What it does

- **Today**: week strip, today's workout from your schedule (or a rest-day card), weekly stats.
- **Workout**: one exercise open at a time, auto-advances when its sets are done. Last session's numbers pre-fill each set; tap ✓ to accept them. Rest timer starts after each set (adjustable in Settings) and beeps when it's over. New personal bests get a PR badge.
- **Finish**: summary with time, sets, total kg moved and PRs.
- **Progress**: workout history plus a trend line and best weight for every exercise.

On iPhone: open the site in Safari → Share → *Add to Home Screen*. Note that the home-screen app keeps its own storage, separate from Safari tabs.

## Editing your plan

Open the **Plan** tab and use this format:

```
# Chest & Triceps @ Mon, Thu     ← workout day, optional weekdays
// Chest                         ← section label
Machine Chest Press | 3x12       ← weighted (kg + reps)
Triceps Dips | 3x10 bw           ← bodyweight (reps only)
Treadmill | 1x30min              ← timed (minutes only)
```

Days with `@ weekdays` show up on the right day; without them the app rotates through your days in order.

## How it's built

```
state (db) ──render()──▶ DOM
    ▲                     │
    └──── update(fn) ◀────┘  user events
```

- One state object is the single source of truth; the DOM is just a projection of it.
- The plan is data parsed from text, not code, so changing your program never means touching logic.
- Derived values (last performance, suggested day, volume) are computed, never stored, so they can't go stale.
- Saved data has a `version` field and a `migrate()` step, so old backups keep importing after schema changes.

## Run locally

```
python3 -m http.server 8000
# open http://localhost:8000
```
