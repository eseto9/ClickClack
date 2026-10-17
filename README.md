# ClickClack

**CLACK** is a one-thumb marble chaos roguelite for phones. Drop a marble anywhere, pull back, let go, and make everything collide. Every collision scores. Reach the room's target to move on.

The whole game is one offline HTML file ([index.html](index.html)) with no assets, libraries or network calls.

## Play

- Open `index.html` in any modern browser, or turn on GitHub Pages (Settings → Pages → deploy from `main`) and open the site on your phone.
- To test on a phone over your Wi-Fi, run `python -m http.server 8000` in this folder and open `http://<your-PC-IP>:8000/` on the phone.
- Add it to your home screen from the browser's share or menu button to play it full screen.

## Controls

| Action | Touch | Keyboard | Gamepad |
|---|---|---|---|
| Place and fling | Touch anywhere to drop the marble there, pull back, release | Arrows/WASD move the drop point, Q/E aim, hold and release Space | Left stick moves, right stick aims, hold and release A |
| Choose the next marble | Tap it in the tray under the board | Number keys 2–9, or Tab | Right/left bumper |
| See your whole bag (pauses) | Tap the bag icon left of the tray | — | — |
| Cancel a shot | Drag back into the ✕ ring and let go, or touch with a second finger | Esc while charging | B while charging |
| Kick (speed up rolling marbles) | Quick tap | K / X / Shift | X or B |
| Fast-forward 3× (after the target is met) | Fast Forward button | F | Y |
| Cash out early | Cash Out button | Enter | Y |
| Pause | Top-corner button | Esc / P | Start |

## How a run works

1. You start with **5 Clay marbles**. Each room deals 5 at random from your bag (the bag icon under the board shows how many you own).
2. Pick which marble to throw next from the tray, then fling it. Marbles that stop stay on the table as targets for later throws.
3. Hit the target score to clear the room, then **pick your path**: two room types to choose from (Gold, Calm, Elite, Wild, Standard). The shop opens after every third room (rooms 2, 5, 8...). After other rooms there's sometimes a free powerup (1 of 3; always after room 1 and boss rooms).
4. If your last marble stops short of the target, the run ends.

**Scoring:** bumpers, blocks and especially marble-on-marble hits score points. Points are multiplied by marble level and material, then by the **Chaos Meter** (×1 → ×2 → ×3 → ×5 → ×8 RIOT), which fills with every hit and drains when things go quiet.

**Upgrades:** common, rare and legendary. 6 materials, 14 engravings (including the legendary Black Hole and Split Shot) and 14 charms (including the legendary Overcharge and Lucky Seven).

**Combos:** 12 named combos (Storm Cell, Chain Reaction, Event Horizon...) trigger when two engravings meet in one collision. Discovered combos fill a codex under Records.

**Arcade board:** big bumpers that light up (light them all for a ×2 Frenzy), ×2 and ×3 gates, boost pads, vortexes, multiball orbs and collectible stars.

**Events:** golden orbs, room modifiers (Slick Floor, Sticky Floor, Super Walls, Blackout, Gold Fever, Bumper Party) and a boss every 5th room with a health bar and telegraphed attacks (The Warden, The Crusher, The Spinner, Juggernaut, The Glutton).

**Modes:** Endless, Daily Seed (same rooms for everyone each day), unlockable Hard mode, and Assist mode (not ranked).

**Friend challenges:** after a run, tap Challenge to send a link with your exact run. Their phone replays it to verify the score, then they race your ghost on the same rooms.

**Progress:** shards from every run, 3 rotating challenges per run, 17 achievements, a Workshop of unlocks (materials, engravings, charm packs, starting bags, perks, themes), a local top 10, and a ghost of your best run to race.

## Settings

Sound and music start **muted**; tap the Sound chip on the title screen or use Settings to turn them on. Settings also has haptics, reduce motion, low power mode (30 fps), aim sensitivity, pause button side, board themes (including High Contrast) and Assist mode.

## Technical notes

- A fixed-step 120 Hz simulation with smoothed rendering. It plays the same on 60, 90 and 120 Hz screens.
- The simulation is deterministic: a run is fully described by its seed plus the recorded inputs. That powers ghosts and the Daily Seed, and `CLACK.verify(record)` replays a record and returns its score (the basis for a future server-checked leaderboard).
- Saves are stored in `localStorage` under `clack.save.v1`, with a migration step for older saves.
- Platform-specific calls (storage, vibration, sharing, keeping the screen awake) sit behind one small adapter so the game can be wrapped as a native app (for example with Capacitor).
- Tuning constants are at the top of the script.
