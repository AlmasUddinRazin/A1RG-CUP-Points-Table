# A1RG CUP – Points Desk v3 (multi-round)

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
