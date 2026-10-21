# ClickClack

**CLACK** is a one-thumb marble roguelite for phones, with a clean, minimal iOS-inspired look: a white card table, flat system-colour bumpers and blocks, and bright marbles you can tell apart at a glance. Drop a marble anywhere, pull back, let go, and make everything collide. Every collision scores. Reach the room's target to move on.

The whole game is one offline HTML file ([index.html](index.html)) with no assets, libraries or network calls.

**Play it:** https://eseto9.github.io/ClickClack/

## Play

- Open the link above on your phone (or open `index.html` in any modern browser).
- To test on a phone over your Wi-Fi, run `python -m http.server 8000` in this folder and open `http://<your-PC-IP>:8000/` on the phone.
- Add it to your home screen from the browser's share or menu button to play it full screen.
- The first time you press Play, a short How to Play screen explains the basics. It's also on the title screen.

## Controls

| Action | Touch | Keyboard | Gamepad |
|---|---|---|---|
| Place and fling | Touch anywhere to drop the marble there, pull back, release | Arrows/WASD move the drop point, Q/E aim, hold and release Space | Left stick moves, right stick aims, hold and release A |
| Choose the next marble | Drag it to the first spot in the tray (or tap it) | Number keys 2–9, or Tab | Right/left bumper |
| See your whole bag (pauses) | Tap the bag icon left of the tray (each marble slowly rotates in the list) | — | — |
| Cancel a shot | Drag back into the ✕ ring and let go, or touch with a second finger | Esc while charging | B while charging |
| Kick (speed up rolling marbles) | Quick tap | K / X / Shift | X or B |
| Fast-forward (after the target is met) | Fast Forward button: 3× speed, room wraps up within 3 seconds | F | Y |
| Cash out early | Cash Out button | Enter | Y |
| Pause | Top-corner button | Esc / P | Start |

## How a run works

1. You start with **4 Clay marbles (one with a random engraving) and a Clay Shooter**. Each room deals 5 at random from your bag (the bag icon under the board shows how many you own).
2. Drag the marble you want to the first spot in the tray, then fling it. Marbles that stop stay on the table as targets for later throws.
3. Hit the target score to clear the room, then **pick your path**: a Standard room, or one special room type (Gold, Calm, Elite or Wild).
4. The shop opens after every third room (rooms 2, 5, 8...). Coins are tight: expect to afford one or two things per visit. After other rooms there's sometimes a free powerup (1 of 3; always after room 1 and boss rooms).
5. If your last marble stops short of the target, the run ends.

Every run uses a random seed, and every engraving, material and charm can show up from your very first run.

**Scoring:** bumpers, blocks, stars and especially marble-on-marble hits score points. **Every hit makes that marble 1% bigger (and heavier) for the rest of the room**, up to 1.6×, so busy marbles grow into wrecking balls that hit more things. Points are multiplied by marble level, material and size, then by the **Chaos Meter** (×1 → ×2 → ×3 → ×5 → ×8 RIOT), which fills with every hit and drains when things go quiet. Early rooms' targets are based on what's on the board, so touching everything gets you there or close.

**Marble sizes:** Peewee (small and quick), standard, Shooter (bigger, heavier, ×1.25) and Boulder (huge, ×1.5).

**Upgrades:** common, rare and legendary. 6 materials, 14 engravings (including the legendary Black Hole and Split Shot) and 14 charms (including the legendary Overcharge and Lucky Seven).

**Combos (the heart of a run):** 28 named combos fire when two engravings meet in one collision, either on the two marbles that hit or both on one marble. Each has its own effect: lightning that chains to nearby marbles, explosions, pulls, freezes, new bumpers, clones, speed bursts, Chaos surges or coins. Upgrade offers lean toward engravings that combo with what's in your bag, and every card and shop item shows which engravings it pairs with (highlighted when you already own one). Discovered combos and what they do fill a codex under Records.

**Arcade board:** big bumpers that light up (light them all once per room for a ×2 Frenzy), ×2 and ×3 gates, boost pads, vortexes, multiball orbs and collectible stars.

**Events:** golden orbs, room modifiers (Slick Floor, Sticky Floor, Super Walls, Blackout, Gold Fever, Bumper Party) and a boss every 5th room with a health bar and telegraphed attacks (The Warden, The Crusher, The Spinner, Juggernaut, The Glutton).

**Friend challenges:** after a run, tap Challenge to send a link with your exact run. Their phone replays it to verify the score, then they race your ghost on the same rooms.

**Progress:** 3 rotating challenges per run, achievements, a local top 10, a combo codex, and a ghost of your best run to race.

## Settings

Sound and music start **muted**; turn them on in Settings. Settings also has Hard mode, haptics, reduce motion, low power mode (30 fps), aim sensitivity, pause button side, themes (Auto follows your phone's light/dark mode, Light, Dark, High Contrast) and Assist mode.

## Technical notes

- A fixed-step 120 Hz simulation with smoothed rendering. It plays the same on 60, 90 and 120 Hz screens.
- The simulation is deterministic: a run is fully described by its seed plus the recorded inputs. That powers ghosts and friend challenges, and `CLACK.verify(record)` replays a record and returns its score (the basis for a future server-checked leaderboard).
- Every room is guaranteed to finish: marbles that keep rolling slow down after 10 seconds, and the board hard-stops 13 seconds after your last throw.
- Saves are stored in `localStorage` under `clack.save.v1`, with a migration step for older saves.
- Platform-specific calls (storage, vibration, sharing, keeping the screen awake) sit behind one small adapter so the game can be wrapped as a native app (for example with Capacitor).
- Tuning constants are at the top of the script.
