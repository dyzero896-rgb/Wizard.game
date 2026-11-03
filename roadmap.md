# Phone play improvements
- [x] Improve movement, aiming, and held touch actions.
- [x] Fit phone menus and add a direct return to play.
- [x] Verify touch combat and menus in landscape and portrait.
# Multiplayer combat roadmap
- [x] Update 1: harden the client network layer without changing spell behavior.
- [x] Update 2: add the lobby, ready check, rounds, winner flow and round shop.
- [x] Update 3: separate game rules into modules and add tests.
- [x] Update 4: keep geometric defensive spell replicas synchronized between players.
- [x] Update 5: validate a small allowlisted set of spell modifiers on every cast.
- [x] Update 6: steer homing projectiles toward live enemy avatars on each client.
- [x] Update 7: confirm hits against locally issued projectile IDs, show hit feedback, and replicate burn/freeze/poison status flags.
- [x] Update 8: route burn and poison deaths through the normal kill path, preserving per-status attacker credit.

The room is still client-hosted. These checks improve message validation and presentation, but do not replace an authoritative game server.
