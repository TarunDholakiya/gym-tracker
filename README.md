# Gym Tracker

A personal workout logger that runs entirely in your phone's browser. No account, no server, no build step: one `index.html`.

- **Works offline**: a service worker caches the app, so it opens at the gym with no signal.
- **Installable**: open the site on your phone → Share / ⋮ menu → *Add to Home Screen*.
- **Private**: data lives in the browser's `localStorage` on your device. Use **Data → Export backup** now and then.

## Editing your plan

Open the **Plan** tab and use this format:

```
# Push
Bench Press | 4x8
Overhead Press | 3x10

# Pull
Deadlift | 3x5
Pull-ups 4x8-10
Plank              // no target = 3 sets, open reps
```

The app suggests the next day in rotation and pre-fills each set with what you did last time. Tap ✓ on an empty set to accept those numbers.

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
