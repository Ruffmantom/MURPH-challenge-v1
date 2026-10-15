# MERPH v1 — Offline-ready challenge tracker

A zero-build progressive web application for individual Murph training and demo group competitions. Plain HTML, CSS, and JavaScript: **no Node install, bundler, or API keys needed**.

## GitHub Pages deployment

1. Create a public GitHub repository (for example `merph-tracker`).
2. Upload **the contents of this folder** to the repository root. Keep `index.html`, `app.js`, `styles.css`, `sw.js`, and `manifest.webmanifest` at the root, and keep the `icons` folder.
3. Go to repository **Settings → Pages → Build and deployment**.
4. Choose **Deploy from a branch**, then select `main` and `/ (root)` and save.
5. Open `https://YOUR-USERNAME.github.io/merph-tracker/` once Pages has deployed. GitHub Pages serves over HTTPS, which is required for installable PWAs.

The manifest and service worker deliberately use **relative paths**, so the PWA works under a GitHub Pages repository subdirectory. You do **not** need to edit any paths to match your repository name.

### Install to your phone

- **iPhone:** Open the GitHub Pages URL in Safari; tap Share → Add to Home Screen → Add. (The wording may vary with iOS version.)
- **Android:** Open in Chrome; use the browser's Install app/Add to Home screen action. The in-app Install button also appears when supported.

After opening it once while online, the app shell is available offline. Saved workouts and profiles stay in that browser's localStorage.

## Local development

Run a simple static web server **inside this directory**:

```bash
python -m http.server 8000
```

Open `http://localhost:8000/`. A PWA/service worker will not work correctly when opening `index.html` directly using `file://`. To test from a different phone use the published HTTPS GitHub Pages URL (localhost on the computer is not localhost on your phone).

## Features

- Mobile-friendly dashboard with progress rings and activity chart.
- Beginner, Advanced, and Pro Murph variants.
- Scaled training targets (25%, 50%, 75%, or 100%); final event always uses the full selected level.
- Live stopwatch with start / pause / resume and persistent elapsed time.
- Optional weighted-vest logging for Advanced and Pro (not required for completion).
- First and last run checkoffs, configurable rounds, manual + / - rep tracking, progress meter.
- Completed session history with personal record indicators.
- Seven-week challenge (six training weeks and a final event), auto-calculated XP.
- Weekly check-in and recovery XP, capped training session XP, no duplicate final-event credit.
- Editable rewards, reward claiming (without deducting XP).
- Multiple **local** participant profiles and simulated leaderboard participants.
- Export and import a JSON backup, manual challenge start date setting.
- Offline service worker, app icons, manifest, iPhone Home Screen metadata.

## Rules used in v1

| Action | XP |
| --- | ---: |
| Complete qualifying training session (up to 3/week) | 20 |
| Third qualifying session in a week | +20 weekly goal bonus |
| One weekly check-in | +10 |
| One weekly recovery activity | +10 |
| Complete final event, including participation, completion, and logged result | 175 |
| Final event reflection recorded | +25 |

Maximum: **100 XP/week × six preparation weeks + 200 XP final = 800 XP**. Sessions outside the configured preparation weeks are saved in History but don't earn training XP. Each profile can earn credit for one final event.

## Important v1 limitation

**This is a single-device prototype.** A different browser, phone, incognito session, or reinstalled app may have separate/cleared local data. There is **no login, cloud database, or cross-device synchronization**. Group rankings are explicitly labeled demonstration data; multiple profiles on the same device can record their own sessions, but these are not shared with other phones. Rewards are locally recorded and not verified or fulfilled by an administrator. Export backups regularly. Never treat client-side XP as tamper-proof competition results.

For real group competitions, the next release should add authentication, a backend (e.g., Supabase), server-side scoring, audit logs, and shared challenge enrollment.

## Editing / update cache

Increment the `CACHE` version in `sw.js` when you publish changes to JS/CSS/assets. The worker pre-caches the new app shell during its install. After publishing, reopen online and reload once to allow the new worker to activate. If necessary, close/reopen the installed PWA.

## Suggested test plan

1. Create a second local profile and switch between profiles.
2. Start the timer, mark both runs, check some rounds, refresh: progress and time should remain.
3. Finish all reps and runs and save a workout; verify History and XP updates.
4. Record recovery/check-in; verify those activities award XP once per week.
5. Claim a reward after enough XP, export data, and import that file.
6. Test offline from the installed Home Screen app after its first successful online visit.
