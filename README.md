# A1RG CUP – Points Desk v4 (multi-round · multi-match · two-way sheet sync)

Files: `index.html` (website on GitHub Pages) · `apps-script/Code.gs` (Google Sheet bridge).

## Update steps (from v2)
1. Google Sheet → Extensions → Apps Script → replace ALL of Code.gs with the new one. Set `ADMIN_KEY` again (same passcode as before).
2. Deploy → Manage deployments → pencil → Version: **New version** → Deploy. (The web-app URL stays the same.)
3. Make sure the Round 2 tab is named exactly **Round 2 - Playing**.
4. In the Apps Script editor run `rebuildNow` once (builds the nice formatted tabs).
5. GitHub: replace `index.html`, commit. Open the site, Ctrl+F5.
6. Settings → **Save & load from sheet**. Use the ROUND 1 / ROUND 2 buttons at the top.

## How rounds work
- Each round has its own roster tab, groups, lobby, results, rules and qualify counts. Switching rounds never touches the other round.
- Round 2 emails are filled automatically from Round 1 (exact name match, or one clear close match). Teams without an email still work.
- Settings → **Round check** shows: qualified Round 1 teams missing from Round 2, extra/misspelled teams, and teams without email.
- DB tabs (Results-DB, Lobby-DB, Rules-DB) have a Round column (blank = Round 1).
- Pretty tabs: `Group-Overall` / `Group-Slots` (Round 1), `R2 Group-Overall` / `R2 Group-Slots` (Round 2).
- Posters/filenames/prompts include the round name.

## Adding Round 3 later
1. Create a tab (same layout as Round 2) e.g. `Round 3 - Playing`.
2. In Code.gs `CONFIG.rounds` add: `{id:3,name:'Round 3',tab:'Round 3 - Playing',emailsFrom:2,prefillNeedsEmail:false}`.
3. Deploy a new version, click Save & load from sheet. A ROUND 3 button appears.

## v4: matches, days and two-way sync
**Matches per group** – Settings → *Round format* (saved to the sheet in `Rounds-DB`). In Match Desk pick the group, then **M1, M2, M3…** and the **Day**. Each match is saved separately; the group overall is the SUM. Tie-break: total → chicken dinners → placement pts → kills → last match finish.
**Standings / Poster** – the "View/Show" box switches between *Overall* and any single match; both can be downloaded as PNG/CSV.
**Sheet tabs** – `Results-DB` has Match + Day columns. `R2 Group-Overall` = sum + per-match totals, `R2 Group-Matches` = every match (appears when a round has more than 1 match).

**Two-way lobby sync (Round 2 and later rounds with `syncSeating:true`)**
- The roster tab (e.g. `Round 2 - Playing`) IS the lobby. Change a team name/slot there → the website updates by itself within ~30 s (or click the "Sheet connected" badge).
- Change slots on the website → saved automatically (≈1 s) and the roster tab is rewritten. A team you take out of the lobby stays selectable under "Not seated".
- `R2 Group-Slots` is only a mirror to read. Edit the roster tab, not that tab.
- Round 1 keeps the old behaviour (its roster tab is not rewritten).

## v4.1
- **Round format** (Match Desk top + Settings): choose Matches per group (1–30) and Days (1–10) for the selected round. Saved to `Rounds-DB`. The Day box in Match Desk offers exactly those days.
- **Standings / Poster "View"**: Overall · Day N (sum of that day's matches) · Match N. The poster header says e.g. `ROUND 2 · DAY 1 · OVERALL STANDING · M1–M2`.
- **Overall tab** (top bar): every team of the round ranked together by the sum of its matches; filter by Group and by Day; Copy/CSV.
- **Group numbers**: a round whose tab numbers groups 1–16 is shown as 33–48 automatically (continues after Round 1). Force with `groupOffset` in `CONFIG.rounds`. If you already saved Round 2 data under 1–16, run `migrateRound2Numbers` once from the Apps Script editor.
- `R2 Group-Slots` lists every group of the round and follows the roster tab; it carries no day/match info.

## v4.2
- **Maps**: Match Desk → Round format → pick the map of every match (Erangel / Miramar / Rondo; any sequence you like, e.g. E/M/R/E/M). Saved in `Rounds-DB` and written into the `Map` cell of every group in the roster tab (Round 2+). Shown in Match Desk, Standings, Overall, posters (footer) and the sheet tabs. With 1 match you can still pick the map. If no map is chosen, the map typed in the roster tab is used.
- **Date / time**: read from the roster tab on every sync (≈30 s or click the "Sheet connected" badge) – edit them in the sheet and they change on the website and posters.
- **Poster pages**: 8 teams per page (16 teams = 2 pages). Choose the page in the Poster tab; "Download all pages" saves both. "16 on one page" keeps the old compact layout. Multi-match overalls show per-match points columns (up to 5 matches).

## v5 – group details, auto-refresh, slot poster
**Update steps:** replace ALL of Code.gs (set `ADMIN_KEY` again) → Deploy → Manage deployments → pencil → New version → Deploy (URL stays the same). Replace index.html on GitHub, then Ctrl+F5.

- **Group details editor** (Match Desk → "Group details"): change Group No., Date, Time and Map rotation on the website and press *Save details to sheet*. They are written into the group's header block on the roster tab (e.g. "Round 2 - Playing"). Changing the Group No. renames saved lobby/results data too. Time is written as `h:mm:00 PM N Match(es)` (N from Round format). Date/Time/Map are written as plain text, so a formula in those cells would be replaced.
- **Auto-refresh:** the site asks the sheet every ~8 s "did anything change?" (tiny `ping` request) and reloads only when it did. It no longer waits for you to click away from a box, and it never overwrites a lobby edit still being saved or details you are typing. Works with an old Code.gs too (slower fallback).
- **Slot poster:** Poster tab → Design → "Lobby slots", or the "Slot poster" button in Match Desk. Info panel (group, date, time, map per match) on the left, slot list #5–#20 on the right, no emails. Choose each match's map, show all matches or one match only, optional note line. PNG downloads as `…-group-33-slots.png`.

## v5.1 – Round 3
- Round 3 added (tab "Round 3 - Playing", groups 49–56, two-way lobby sync like Round 2). Matches/days start at 2 / 1; change them in Round format any time.
- Group details: Map rotation now has one dropdown per match (Match 1 Erangel, Match 2 Rondo …) and is written as `Erangel / Rondo`.
- Round 3 look: Blood Moon backdrop, red accent, hazard-stripe frame + corner brackets ("Elite frame" checkbox in Poster). New backdrops: Blood Moon, Neon Teal. Each round remembers its own backdrop/colour.
- Update: replace all of Code.gs (set ADMIN_KEY), Deploy → New version; replace index.html; Ctrl+F5.

## v6 – Broadcast result poster + Drive team photos
**Update:** replace all of Code.gs (set `ADMIN_KEY` again), set `PHOTO_FOLDER_ID` (below), Deploy → Manage deployments → pencil → New version. Replace index.html, Ctrl+F5.

**Poster tab → Layout "Broadcast poster 4:5"** (1440×1800): big stage title (e.g. SEMI FINALS), "OVERALL RANKING · GROUP 49", hero panel (photo of one team + WWCD / PLACE PTS / ELIMS / TOTAL PTS + #rank + team name), 8+8 two-column table, "WATCH ACTION LIVE ON" + qualify line, date/time/map and credit. More than 16 teams → extra pages. The old 16:9 layout is still there ("Wide"). New backdrops: Violet Gold, Navy Red.

**Drive photos (fetched one team at a time):**
1. In Google Drive create a folder (e.g. "A1RG Team Photos") with the same Google account that owns the Apps Script. Open it and copy the id from the link: `drive.google.com/drive/folders/THIS_PART`.
2. Paste it in Code.gs: `PHOTO_FOLDER_ID: 'THIS_PART'`. In the Apps Script editor choose the function `photoList_` and press Run once, and accept the Drive permission. Then redeploy as a New version.
3. Put one picture per team in that folder, named after the team: `kh-esports.png`. Capitals, spaces and symbols do not matter (`KH Esports.png` works too) – the name just has to match the team name in the sheet. Optional logo files `kh-esports-logo.png` are already recognised by the server but not drawn on the poster yet.
4. Picture tips: a wide squad photo, about 1600×500 px, players cut out on a transparent PNG (or a normal JPG), feet at the bottom edge, under 6 MB (1–2 MB is ideal).
5. On the poster the site automatically asks Drive only for the team in 1st place. The "Team photo" box lets you pick any other team or "No photo". Added a new photo while the site is open? Press "Refresh photo list from Drive".

## v6.1 – your Drive folders, player pictures, winner logo
The three folders you made are already in Code.gs: `PHOTO_FOLDER_ID` = Team Photos, `LOGO_FOLDER_ID` = Team Logos. The script must be owned by the same Google account as the folders (almas.razin.official@gmail.com). After pasting Code.gs: run the function `photoList_` once in the editor and accept the Drive permission, then Deploy → New version.

**What to upload**
- **Team Logos:** one file per team, named after the team: `kh-esports.png` (capitals/spaces/symbols do not matter). Square or round PNG with transparent background works best.
- **Team Photos, option A – squad photo:** one file per team directly in the folder: `kh-esports.png`.
- **Team Photos, option B – individual players (recommended):** make a sub-folder named after the team (`KH Esports`) and put one picture per player in it, file name = player name (`Sabbir.png`, `Rafi.png`). Put a number first to fix the order: `1 Sabbir.png`, `2 Rafi.png`. Transparent PNG cutouts, head to waist, about 700×1000 px, under 6 MB each. Any number of players is fine.

**On the poster:** Photo type = "Individual players" shows a tick-box per player of the 1st-place team; tick the 4–6 you want (the first 5 are ticked by default). They are placed side by side with the middle one in front. The team logo appears next to the team name in the hero panel. Only the pictures of the team that is shown are downloaded. "Refresh photo list from Drive" picks up newly uploaded files (otherwise the list refreshes itself every 5 minutes).

## v6.2 – photo frames + per-player editing
Poster tab → Photo type "Individual players":
- **Photo frames:** "Equal photo cards" (default, every player in a card of the same size – best for pictures with a background) or "Cut-outs side by side" (transparent PNGs standing next to each other).
- **Edit positions** (tick the box): click a player on the poster, drag to move the picture inside its card, mouse wheel (or the Size slider) to zoom. The sliders under it move Left/Right and Up/Down precisely. "◀ Earlier / Later ▶" changes the player's place in the row. "Reset this player" / "Reset all" go back to automatic. The blue outline is only on screen, never in the downloaded PNG.
- Adjustments are remembered per team and per player on this device/browser.

## v6.3 – no gaps, logo editing, AI background remover
- **Photo cards:** the last card now runs right up to the slanted edge, so there is no empty strip next to the stats.
- **Logo:** in "Edit positions" mode the team logo can be selected, dragged, zoomed and nudged like a player. Tick/untick "White box behind the team logo" in the Poster form.
- **Background remover:** select a player (or the logo) in "Edit positions" mode and tick "Remove the background of this picture", or press "Cut out all players". It runs inside your browser with the open-source IMG.LY model (AGPL licence). The first time it downloads the model (about 40 MB, needs internet) and takes a minute; after that each picture takes a few seconds. Finished cut-outs are kept on this device, so they are instant next time. "Save this cut-out" downloads the transparent PNG, which you can upload to the Drive folder instead of the original, so the cut-out is the same on every computer. Cut-out players get an accent-coloured card background.

## v7 – Tournament logo, Round 4, faster
- **Tournament name:** `tournament` in Code.gs CONFIG (now "A1RG COMMUNITY STARS"). The website header, browser tab title and all posters follow it. Change it there if the spelling should differ.
- **Tournament logo:** Poster tab → Logo → "Tournament logo" (default; replaces the PUBG logo). On a light backdrop the white logo is turned dark automatically. Size / left-right / up-down sliders, and on the Broadcast poster you can also drag it ("Edit positions"). "Own logo link" lets you paste any other image link (use the raw link, e.g. https://raw.githubusercontent.com/...). Choose "PUBG Mobile Esports" in the same box to get the old logo back.
- **Round 4:** tab "Round 4 - Playing", groups numbered as written in the tab, same two-way lobby sync, editable group details, own look (Violet Gold). Set the number of matches/days in Round format.
- **Speed:**
  - Sheet: the server reads each roster tab once per request (was 2–3 times), and many open browsers share one 5-second change check.
  - Drive: the website asks for the list of pictures of ONE team (names + version), and downloads a picture only if this device has never seen that version. Pictures are stored on the device (IndexedDB), so the next poster is instant. Replaced pictures are noticed automatically.
  - Website: the poster form (14 backdrop previews) is no longer rebuilt on every sync, only when the Poster tab is open and the colour changed.
- Update: replace all of Code.gs (set `ADMIN_KEY` again) → Deploy → Manage deployments → pencil → New version. Replace index.html, Ctrl+F5.
