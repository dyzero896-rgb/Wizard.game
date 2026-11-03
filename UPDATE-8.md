# Update 8 of 8 — Status-effect deaths

## Runtime changes

- Burn and poison ticks no longer subtract player HP directly inside the frame loop.
- Each tick is applied through the existing `hurtPlayer()` chain, so direct spell hits and damage-over-time share hit feedback, last-hit tracking, downed state, respawn, and multiplayer death handling.
- Burn and poison retain separate source-player IDs. A lethal burn tick credits the burn source; a lethal poison tick credits the poison source.
- Status sources are cleared when the effect expires or the player is reset, respawned, or enters a rebuilt arena.
- A death event is emitted only for a positive-to-zero HP transition, preventing duplicate kill events after the player is already down.

## Existing balancing preserved

- Burn: 1.5 damage every 30 frames.
- Poison: 1 damage every 30 frames.

## Validation

- `node --test tests/game-modules.test.mjs`: 21 tests passing.
- Inline game module syntax check: passed.
- A full Vercel production build was not run because dependencies are not installed in this environment.
- No live two-player browser match was run. The realtime room remains client-hosted; these changes do not provide server-authoritative combat or anti-cheat.
