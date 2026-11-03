# Lobby repair follow-up

This patch follows the Step 8 reliability fixes and specifically addresses reports that the initial screen, guest flow, and room creation felt non-functional.

## Changes

- The lobby now uses balanced desktop columns and starts at the top of its scroll container, avoiding clipped/awkward centering on tall screens.
- Mobile ordering now presents identity/room controls before mode/map options.
- Guest and local-profile selection now complete an explicit local identity step and show a compact identity badge with a Change identity action.
- The display name is sanitized and consistently used in room presence.
- The unused Public room selector was removed; this prototype supports invite-code rooms only.
- Room codes use a dedicated tested helper and the accepted `XXXX-XXXX` format.
- Room creation and joining now await the Supabase Realtime channel subscription. The UI does not call a room live until subscription succeeds; failures and timeouts are visible and the code is marked inactive.
- Supabase SDK loading has an alternate CDN attempt. Failure is caught and shown as a multiplayer connection error, while local practice remains available if the game engine itself is loaded.
- Duplicate legacy join/leave handlers and the old automatic invite join were removed; invite links pre-fill the room code and ask the player to click Join room.
- The layout no longer includes the nonfunctional “Public room (local demo)” option.

## Validation performed

- `node --test tests/*.test.mjs`: **28 tests passed**.
- `node --check`: all `public/game-modules/*.js` modules and both inline module scripts passed.
- No duplicate element IDs were found in `public/game.html`.
- Static source checks confirm one primary Join room click handler and one Leave room click handler.
- ZIP integrity was checked after packaging.

## Not yet verified

- A real browser interaction test could not be run in this environment.
- `npm run build` could not run because dependencies are absent (`vite: not found`). Run the project's package install, then `npm test` and `npm run build` before deployment.
- The password field remains a local-only profile/demo field. It does **not** authenticate a real account. Proper username/password accounts require a configured auth provider/server.
- Real room creation depends on loading the Supabase SDK and reaching the configured Lovable Cloud Realtime endpoint. The UI now reports this failure instead of claiming success when it has not connected.
