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
