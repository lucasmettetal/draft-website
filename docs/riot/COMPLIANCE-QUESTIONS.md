# DRAFT — Questions for Riot Games

*Open questions to raise with Riot (Developer Portal application or app messaging). No answer is assumed; each feature can be changed or removed depending on Riot's guidance.*

## Champion select assistance

1. **Ranking of the player's own pool.** DRAFT ranks the champions a player has saved against the enemy composition and highlights a first choice and an alternative, with reasons. Riot's policy says products "should not remove game decisions, but may highlight decisions that are important and give multiple choices" and that apps which "dictate player decisions" are not approved. Is a ranked list with explanations acceptable? Should DRAFT avoid naming a single "recommended" pick (e.g. show tiers or the top 2–3 only)?
2. **Educational tips during champion select.** Hand-written tips (win condition, threats, targets, suggested bans, phase advice) are shown during the draft. Are suggested bans and in-draft tips acceptable?
3. **Enemy role guessing.** The client does not expose enemy positions. DRAFT guesses them from aggregated role statistics. Is this considered "information not present in the game client"?

## League Client API

4. **Optional read-only LCU access during champion select** (`/lol-gameflow/v1/gameflow-phase`, `/lol-champ-select/v1/session`, GET only, off by default) to fill in enemy picks that are already locked in and the player's assigned position. Is this acceptable, and how should it be declared on the product page?
5. **`assignedPosition`** of the local player is used to suggest the matching role profile. Is relying on this field acceptable given it is undocumented?
6. **Screen thumbnail.** To open its panels automatically, DRAFT reads a 96×54 px in-memory thumbnail of the client window area to detect the champion select screen (no OCR, nothing stored). Is this acceptable, or should detection rely only on the LCU phase?

## Riot API data

7. **Personal statistics import** (ACCOUNT-V1 + MATCH-V5): is Riot Sign On required for a Production key, so that players can only import their own accounts?
8. **Aggregated statistics** (LEAGUE-EXP-V4 as a random seed + MATCH-V5): counting champion positions and matchup records across random ranked players, storing only anonymous counters. Is this an approved "aggregate stats with no specific players" use case? Are there constraints on which tiers to sample?
9. **Matchup records used in recommendations.** Computed matchup win-rate gaps feed one ranking criterion when at least 30 games are available. Any restriction on using win rates in a champion select tool?

## Distribution

10. **Windows desktop app.** With a Production key held only on a DRAFT server (never in the binary), is distributing a free Windows executable acceptable? Any requirement on code signing or update mechanism?
11. **Beta phase.** Until a Production key is granted, is it acceptable for a handful of private testers to use their own development keys for the optional API features, or should those features be disabled for testers?
12. **Future public release.** Which changes (new features, LCU use, monetization—currently none) require a new audit through the product page?
