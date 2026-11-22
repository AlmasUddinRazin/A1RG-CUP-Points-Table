# A1RG CUP Points Desk – setup (10 minutes)
 
## 1. Google Sheet bridge
1. Open your tournament Sheet → Extensions → Apps Script. Paste `apps-script/Code.gs`.
2. Change `ADMIN_KEY` (and scoring/qualify in `CONFIG` if needed). Save.
3. Deploy → New deployment → Web app → Execute as **Me**, access **Anyone** → copy the `/exec` URL.
   (Re-deploy as a *new version* after any later edit to Code.gs.)
The script reads `Groups - Playing` automatically and creates `Lobby-DB`, `Results-DB` and a clean `Group-Overall` tab (one fixed block per group, rebuilt on every save). Your old tabs are not touched.
## 2. Website (GitHub Pages)
1. Put `index.html` in a repo → Settings → Pages → deploy from main.
2. Open the site → Settings tab → paste the `/exec` URL + passcode → Save & load. Do this once per admin device.
3. Pictures load from `website-assets` via raw.githubusercontent.com – that repo must be public.
   Tip: rename the files to `bg1.jpg` / `bg2.jpg` (no spaces/brackets) and update the `IMG` constants at the top of `index.html`.
## 3. Match flow
Group → seat teams (search) → Copy message for Claude → paste in Claude + screenshots → paste Claude's table back → Check → Save.
Ranking: total points → chicken dinner → placement pts → kills → best finish.
 
