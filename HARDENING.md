# Online play: hardening notes

Online matches are **peer-to-peer**. Each browser simulates its own player and sends
its position, casts and hits to the others through Supabase Realtime. There is no
server deciding outcomes, so a modified browser can still misbehave. What this
release does is narrow what a forged packet can achieve.

## What is enforced now (`public/game-modules/hardening.js`)

| Threat | Rule |
|---|---|
| Huge claimed damage | Each projectile is clamped to the spell table (never above 60); combined damage in one cast packet is scaled to at most 35. |
| Flooding damage | Each sender has a damage budget: 90 burst, refilling at 45 per second. |
| Shells spawned on you | A cast's shells must start within 9 units of the sender's last known position. Casts from unknown senders are dropped. |
| Teleporting | Avatars may move at most 60 units per second between packets (respawn excepted). |
| Forged prep timer | Other players' prep deadlines are only believed if they fall within the next 35 seconds. |
| Forged readiness | Readiness is read from presence only; ready broadcasts are ignored. |
| Host takeover | The host is pinned for the match. Only the pinned host can publish match results. |
| Round rules | Last team standing ends a round. A round timer decides by survivors, then total health. |
| Forfeits | If one team has nobody in the room for 4 seconds, the other team wins. |
| Respawn | Eliminated players stay down until the next round. |
| Shop | Upgrades last for one match. They are derived from defaults, so they cannot stack across matches. Leaving a room restores your own balance settings. |
| Balance sliders | Locked while in an online room. |

## What is not solved

- A cheating client can still lie about its own health, position within the limits above,
  and which spells it owns. Other clients can only spot gross mismatches.
- Kills are still reported by the victim's browser. A determined cheater can still
  fake kills on its own side.
- Real anti-cheat needs a server that simulates combat. Vercel functions cannot hold
  live game connections, so that needs a separate service (for example a small WebSocket
  server or PartyKit / Colyseus).

## Other changes

- `vercel.json` adds security headers. The Content-Security-Policy is **report-only** for now,
  so it cannot break the game; check the browser console for reports, then enforce it.
- The realtime SDK and Three.js have local fallbacks in `public/vendor/`, so the lobby still
  works if the CDN is unreachable.
- The unused lobby password field was removed. Accounts are not implemented.

## Tests

`npm test` runs the Vitest suite, the module tests, and the integration tests in
`tests/integration/`. The integration tests load the real `game.html` in jsdom with a fake
renderer and fake realtime channel. They cover forged casts, per-packet damage caps, touch-device round entry, damage budgets, teleports,
prep hijacks, host pinning, last-team-standing, forfeits, respawn lock, shop scope, and
malformed packets.
