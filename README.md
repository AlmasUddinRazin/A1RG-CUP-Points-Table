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
