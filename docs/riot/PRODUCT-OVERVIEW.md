# DRAFT — Product overview

*Prepared for a Riot Games Developer Portal application. Describes DRAFT 2.16 beta as verified in the source code (branch `feature/draft-profiles-beta` of [lucasmettetal/Draft](https://github.com/lucasmettetal/Draft); the public `main` branch is 2.14). DRAFT is not registered with or approved by Riot Games.*

## Purpose

DRAFT is a free Windows companion app for League of Legends champion select. A player saves the champions they play for each role. When the enemy team's picks are entered, DRAFT ranks those champions against the enemy composition, highlights the best fits with an alternative, and explains the reasoning and a game plan. The player always picks, bans and locks in themselves.

## Audience

Solo queue players, especially beginners and intermediate players with a small champion pool, who want to understand why a pick fits a composition rather than copy a tier list.

## Features and maturity

| Feature | Status in 2.16 beta |
|---|---|
| Ranking of the player's pool against the enemy composition (rules + hand-written champion sheets) | Working |
| Explanation: criteria table, ties flagged when two picks are very close | Working |
| Game plan and educational tips (laning, mid game, teamfights; beginner/intermediate levels) | Working, content still under review |
| Compact panels placed next to the client window | Working |
| Automatic panel opening when champion select is detected (96×54 px in-memory thumbnail of the client window) | Experimental, on by default, can be disabled |
| Five role profiles with independent pools, export/import as JSON | Working (2.16 beta) |
| Manual entry of enemy champions, one-click correction of their guessed role | Working |
| Optional League client reading (LCU), read-only | Experimental, off by default |
| Suggestion of the profile matching the position assigned by the client | Experimental, needs LCU reading |
| Personal ranked history import (Riot API) | Working, user-triggered |
| Aggregated role statistics per champion (Riot API) | Working, user-triggered, ~40 min |
| Pool matchup statistics (Riot API) | Working, user-triggered, ~65 min |

## User journey

1. Launch `DRAFT-Portable.exe` (no installer required). An icon appears in the Windows tray.
2. Open settings, choose a role, add the champions played.
3. During champion select, open the panels (hotkey, tray menu or automatic opening).
4. Type each enemy champion as it is locked in, or enable client reading to fill them in.
5. Read the ranking, the alternative, the reasons and the game plan, then pick in the client.
6. Optionally: import ranked history, or compute role/matchup statistics, by entering a Riot API key.

## Technology

- C# (.NET Framework 4.x), Windows Forms tray app, single executable.
- UI in an embedded Microsoft Edge WebView2 (HTML/JS bundled in the executable).
- All data stored locally in `%LOCALAPPDATA%\DraftMid`. No DRAFT server, no telemetry.

## Data sources

- Hand-written champion sheets and rules bundled with the app.
- Riot Data Dragon: champion artwork and versions.
- Riot Games API (optional, user key in beta): ACCOUNT-V1, MATCH-V5, LEAGUE-EXP-V4. See [API-USAGE.md](API-USAGE.md).
- League Client API (optional, local, read-only): game flow phase and champion select session.

## What DRAFT does

- Ranks the player's own champions and explains several options.
- Shows information already visible to the player in champion select (enemy picks that are locked in) plus general per-champion statistics.
- Keeps everything on the player's PC.

## What DRAFT does not do

- No picking, banning, locking in, or any in-game action; no input automation.
- No reading of hidden information (enemy hovers, other players' names or IDs).
- No display, storage or ranking of other players; no MMR or rank estimation.
- No memory reading, injection or network interception of the client.
- No ads, no paid features, no accounts.

## Current limitations

- Interface in French only.
- Educational tips are hand-written and not yet validated by a panel of players.
- Enemy roles are guesses; the client does not expose enemy positions.
- Beta testers would each need their own development key for API features; the history import does not verify account ownership (no RSO).
- Client reading relies on the League Client API, which Riot does not officially support for third parties.
- Executable not code-signed; tested on Windows 10/11 only.
- 2.16 beta not yet distributed to testers.
