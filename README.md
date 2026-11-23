# A1RG CUP Points Desk – setup & update guide

## Updating from the previous version
1. Apps Script: replace ALL of `Code.gs` with the new file (re-set `ADMIN_KEY`). Deploy > Manage deployments > pencil > Version: **New version** > Deploy. The URL does not change.
2. Run `rebuildNow` once from the editor (function menu > rebuildNow > Run, allow permissions if asked). It builds the formatted tabs from the results you already saved.
3. GitHub: replace `index.html`, then hard-refresh (Ctrl+F5).

## Tabs in your Google Sheet
| Tab | For | Notes |
|---|---|---|
| Groups - Playing | you | your roster, never touched |
| **Group-Overall** | humans | one header box per group, groups in order, green = qualified, red = out |
| **Group-Slots** | humans | one header box per group with the lobby |
| Results-DB / Lobby-DB / Rules-DB | the website | grey data tabs. Do not edit by hand |

## Match flow
Group → seat teams (drag or search) → Copy message for Claude → paste into Claude with screenshots → paste Claude's table back → Check → set "Teams qualifying" if different → Save.

## Qualify count & scoring
- "Teams qualifying" can be changed per group in Match Desk, Standings and Poster. The sheet updates automatically.
- Settings > Scoring: default for all groups, or custom rules for one group (placement points, kill points, qualify count). Shared with all admins.
- Ranking: total points → chicken dinner → placement pts → kills → best finish.

## Pictures
Loaded from `website-assets` via raw.githubusercontent.com (repo must be public). Poster > Picture layout: Auto / Full background / Right-side cutout / Right half.
