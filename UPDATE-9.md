# Update 9 — mobile arena entry and safer 1v1 damage bursts

## Fixed

- Coarse-pointer/mobile browser detection now also checks `navigator.maxTouchPoints`, rather than relying only on `pointer: coarse` and the legacy `ontouchstart` property.
- Touch devices open in the Wand Lab edit state with the **Return to arena** control available.
- Receiving an online `round` transition now enters touch-play mode on phones, so the game stage is not left hidden behind the Wand Lab.
- Incoming cast packets are scaled as a group to a maximum of 35 damage, preventing a single multicast packet from instantly defeating a full-health player.
- Per-sender damage budget reduced to a 90-damage burst with 45 damage/second refill.
- Damage budget storage now uses a null-prototype object; reserved prototype property names are rejected as player IDs.

## Verification

- Added unit tests for mobile-device detection and combined cast-packet damage.
- Added integration regression tests for phone round entry / return-to-arena edit mode and a maximum-size lethal multicast packet.
- Run `npm test` with dependencies installed and verify the 1v1 flow again on both a physical phone and a desktop browser. This patch has not been live-tested against the deployed Supabase project.

## Multiplayer note

These limits reduce accidental one-cast eliminations and client-side abuse, but they do not make the peer-to-peer room authoritative or cheat-proof. A trusted server is still required for competitive matchmaking.
