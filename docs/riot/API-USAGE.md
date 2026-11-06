# DRAFT — Riot API usage

*Exact inventory of Riot calls in DRAFT 2.16 beta, verified in `src/HistoryImporter.cs`, `src/RoleRatesUpdater.cs` and `src/LcuConnector.cs`. "Current" = implemented. "Planned" = not built.*

## Current architecture

- Desktop app, no backend. Each user who wants an API feature enters **their own development key** in a dialog. The key is masked, kept in memory for the operation, sent only in the `X-Riot-Token` header over HTTPS, never stored, never logged, never put in a URL.
- All three API features are **user-triggered** (settings or tray menu). Nothing runs in the background on its own.
- Pacing: one request every **1.3 s** at most (below 100 requests / 2 min). **429**: wait for `Retry-After` (120 s if absent), then resume. **401/403**: stop with a message. Other errors: up to 5 retries 2 s apart, then stop. Progress is checkpointed; matches already read are never requested again.

## 1. Personal history import (current)

| Step | Endpoint | Purpose |
|---|---|---|
| 1 | `GET {region}.api.riotgames.com/riot/account/v1/accounts/by-riot-id/{gameName}/{tagLine}` | Resolve the Riot ID entered to a PUUID |
| 2 | `GET {region}/lol/match/v5/matches/by-puuid/{puuid}/ids?queue=420&startTime=…&start=…&count=100` | List ranked solo/duo match IDs |
| 3 | `GET {region}/lol/match/v5/matches/{matchId}` | Read each match |

- **Input:** one or more Riot IDs (comma-separated), region, 100/200/500 games per run. Intended for the user's own accounts; **ownership is not verified** in the beta.
- **Kept locally** (`draft-history.js`, until the user deletes it; 5 dated backups): Riot ID entered; per match: match ID, creation time, patch, duration, champion, position, win, K/D/A, enemy champion names, enemy mid champion. Remakes (< 5 min) and other queues are skipped. **No other player's name, Riot ID or PUUID is stored.**
- **Shown:** most played champions, win/loss per champion and per enemy mid type (Games tab), "most played" hint next to the recommendation. History never changes the ranking.
- **Volume:** 1 + ⌈N/100⌉ + N requests for N games (≈ 102 for 100 games, ~2–3 min).

## 2. Aggregated role statistics (current)

- **Goal:** estimate which position each champion is played in, to guess enemy roles (notably mid).
- **Calls:** `GET {platform}/lol/league-exp/v4/entries/RANKED_SOLO_5x5/{SILVER|GOLD|PLATINUM|EMERALD}/II?page=1` (4 calls) to get a pool of random players → `match-v5 …/ids?queue=420` (5 matches per player, last 14 days) → `match-v5 …/matches/{id}`.
- **Target:** 1,000 matches (~40 min). Skipped if a computation with enough games is less than 3 days old.
- **Kept** (`draft-role-rates-live.js` + state file): champion × position counters per patch, IDs of matches read, a hold-out sample of team compositions (champion names only) to measure guess accuracy. Player PUUIDs from LEAGUE-EXP-V4 are **in memory only**.
- **Shown:** guessed role of each enemy champion; the date and game count in settings.

## 3. Pool matchup statistics (current)

- **Goal:** for the "direct matchup" criterion, measure each pool champion's record against its same-position opponent.
- **Calls:** same LEAGUE-EXP-V4 seed, then `match-v5` lists/details. Players seen playing a pool champion are followed (their recent ranked match IDs, last 30 days) **in memory** to find more games of that champion.
- **Cap:** 3,000 requests (~65 min), 800 games targeted per pool champion.
- **Kept** (`draft-matchups-live.js` + state file): per patch, per pool champion: wins, games, and wins/games against each opponent; IDs of matches read. No player data.
- **Use:** below 30 games for a matchup, the computed record is ignored and the hand-written table is used. Records are shrunk toward the champion's average and only the gap to that average is used (followed players tend to win more).
- **Shown:** matchup criterion in the Analysis tab; game counts in settings.

## Not the Riot Developer API: League Client API (current, optional)

- Local HTTPS calls to `127.0.0.1:{port}` with the credentials from the client's `lockfile` (password not stored). **Off by default.**
- **Whitelist enforced in code, GET only:** `/lol-gameflow/v1/gameflow-phase`, `/lol-champ-select/v1/session`. Any other path throws.
- **Used fields:** phase; enemy champions that are locked in; `assignedPosition` of the local player's cell. Ignored: names, IDs, `championPickIntent` (hovers), allied picks, bans.
- `assignedPosition` is undocumented and was only tested with simulated responses.

## Static data

- Data Dragon: `ddragon.leagueoflegends.com/api/versions.json`, champion square and splash images. No key.

## Planned architecture with a Production key (not built)

1. A DRAFT server holds the Production key; the desktop app ships **no key** and never asks users for one.
2. The server computes role and matchup statistics **once per patch/region** and publishes anonymous counters (no player data), which the app downloads. This replaces thousands of per-user requests.
3. Personal history import moves to **Riot Sign On**: the user signs in, the server fetches only that account's ranked matches and returns the same minimal fields; nothing about other players is kept.
4. Server-side rate limiting, caching of match details, and key storage in a secret manager.
5. The app keeps working fully offline for the core ranking if the server is unavailable.

Estimated steady-state volume (to validate): a few thousand match-detail requests per patch per region for aggregates, plus per-user history imports on demand.
