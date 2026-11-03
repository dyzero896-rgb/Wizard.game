# Update 10 — keep players in the arena between rounds

## Reported issue

In a two-player room, the game sent both guests to the lobby as soon as a player reached 0 HP.

## Change

- `intermission` and `finished` are now in-arena states. The game no longer sets `NET.inArena` to false or calls `openLobby()` when a round ends.
- Added an in-arena round-end panel showing the match score and readiness status.
- Both players can ready up from the panel. The host can start the next round, or start a rematch after the match is finished.
- The between-round shop is available on the panel during intermission.
- A defeated player remains at 0 HP while the round-end panel is shown. The next round's normal spawn resets health to 100.
- Player movement, firing, defense casting, incoming casts and hit confirmations are paused during intermission/finished states.
- Pending projectiles and active defenses are cleared at round end.
- The Supabase room channel and presence remain connected.

## Verification

- Native Node test suite: 39 passed.
- JavaScript syntax checks: passed for both inline game scripts and the changed integration test.
- HTML IDs: 104 unique; 13 relative game-module imports exist.
- Integration tests were updated for arena-preserving intermission and frozen 0 HP, but could not be executed in this environment because `jsdom` is unavailable and its install timed out.
- No live two-device Supabase test was available in this environment.
