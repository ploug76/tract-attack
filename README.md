# Tract Attack (Book of Heaven Studios)

Live: https://ploug76.github.io/tract-attack/  ·  Free sample: https://ploug76.github.io/tract-attack/?sample=bhs

Hand out Bible tracts, send glowing prayers that turn little gloom goblins into light, and set gloomy people free.
5 levels: 1 Main Street Morning (side), 2 Sunny Park (side, flying goblins), 3 Neighborhood Walk (top-down, gloom pits),
4 Downtown at Night (side), 5 Farmers Market (top-down, gloom pits + the Big Grumble at the chapel door).

## Controls (NES-style pad along the bottom; on the sides in landscape)
- D-pad: walk. Side levels: ▲ jump, ▼ drop off a bench. Top-down: walk any way.
- B = TRACT: hand a tract to someone close or toss one to them (+100). Hold to keep tossing.
- A = PRAY: a glowing prayer orb. Goblins dissolve into light (+50). Gloomy people are set free (+300, never hurt).
- Keys: arrows/WASD, Z = tract, X = pray, Space = jump, P pause, M mute.
- Title: Gentle mode (5 hearts, slower/fewer goblins), Music on/off, Levels (unlocked levels are saved).

## Source / build
- Source: `src/1_core.js` (layout, audio + original soft music), `2_art.js` (hero, people, goblins), `3_world.js` (levels, maps, backgrounds),
  `4_play.js` (gameplay), `5_ui.js` (input, controller, HUD, screens), `6_qa.js` (test-only hooks/autopilot; inert unless a test sets `window.__QA`).
- Build: `python3 /workspace/tract-attack-src/build.py` → writes /workspace/tract-attack (index.html, sw.js, manifest, icons, covers).
- **Bump `src/version` on every push** (that number is the sw.js cache version).
- Commit in /workspace/tract-attack with `git -c user.name=ploug76 -c user.email=ploug76@users.noreply.github.com`, pull before push, never force-push.
- Share/sample: entry at the end of /workspace/launch/share.py (before the runner): share_sample='bhs', CAP = levels 1–2 (locks on the level 3 intro; 8-minute backup). build.py asserts it.
- Leaderboard: shared Firestore board `tract-attack` (games.py entry is set in memory by build.py). Name prompt pops on game over / mission complete. Tests never post (`window.__QA`).

## Swapping the hero (when Peter's photo arrives)
The hero is a placeholder ("Pete", blue cap, white tee with a gold cross, tract satchel). Everything about his look is the `HERO` object
at the top of `src/2_art.js`:
- `head` = 10x10 side view facing RIGHT, `headF` = front view, `headB` = back of head. Each letter is a colour in `HERO.pal`; `.` is see-through.
- `shirt`, `shirt2`, `pants`, `shoes`, `bag`, `skin` colour the body.
To use the photo: make a 10x10 pixel likeness with the J Woo method (/workspace/jwoo-likeness/), quantize it to a few colours,
write those rows into `head`/`headF` (keep 10x10), set the colours in `pal`, then rebuild, bump `src/version`, retest and push.
The game never embeds the real photo. Nothing else needs to change: animation, flipping and throwing all use the same maps.

## Tests (serve /workspace: `cd /workspace && python3 -m http.server 8890`)
- `test/smoke.py`: title + levels at 390x844, iPad portrait 820x1180 and iPad landscape 1180x820; console errors.
- `test/playall.py`: autopilot plays all 5 levels (speed x4). `test/sample.py`: sample cap + Share link. `test/capture.py`: real screenshots for covers.
- Launch tools: `python3 /workspace/launch/dock_test.py tract-attack` and `python3 /workspace/storefronts/sample_test.py`.
