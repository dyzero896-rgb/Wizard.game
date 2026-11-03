# Wizzardo

im creating this wizard game take a deep look and make some improvments

This project was built with [Lovable](https://lovable.dev).

**Live app**: https://wizzardo.lovable.app

## Build with Lovable

Continue developing this project in the [Lovable editor](https://lovable.dev/projects/97959203-3041-40ae-8648-7996a7a3da1c).

- **Ship faster**: describe what you want to build and Lovable handles the code.
- **Stay in sync**: every change made in Lovable is committed straight to this repository.
- **Full ownership**: this code is yours. Push to `main` on GitHub and your changes sync back into Lovable, ready for your next prompt.

## Development

Prefer working locally? You need Node.js and npm — [install with nvm](https://github.com/nvm-sh/nvm#installing-and-updating).

```sh
git clone <this-repository-url>
cd <repository-name>
npm i
npm run dev
```

## Vercel deployment

This repository is configured for the TanStack Start framework on Vercel (`vercel.json`). Import the repository into Vercel and use the repository root as the project root. Set the following variables in **Project Settings → Environment Variables** for Production, Preview, and Development as appropriate:

- `SUPABASE_URL`
- `SUPABASE_PUBLISHABLE_KEY`
- `SUPABASE_PROJECT_ID`
- `VITE_SUPABASE_URL`
- `VITE_SUPABASE_PUBLISHABLE_KEY`
- `VITE_SUPABASE_PROJECT_ID`

Use only the Supabase publishable/anon key in browser-visible variables. Never set a service-role key under a `VITE_` name or put it in `public/game.html`. `.env` files are excluded from the deployment archive; `.env.example` documents the required names. After adding variables, redeploy.

### Multiplayer network hardening (step 1)

The browser now validates incoming presence metadata and state/cast/defense/kill packets, bounds coordinates and projectile values, limits packet rates per known room member, sanitizes display names/colors/mode/map values, and handles broadcast/presence send errors without breaking the render loop. This is client-side defensive validation only: because the current game has no authoritative match server, a modified client can still spoof its own packets. Do not treat it as anti-cheat or authoritative damage validation.


### Game module structure (step 3)

`public/game.html` remains the standalone game entry point. Pure modules that can be verified independently now live under `public/game-modules/`:

- `spell-catalog.js` contains the original offensive spell data object; no spell values are tuned in this refactor.
- `defense-catalog.js` contains the original defensive spell data.
- `wand-presets.js` contains the original named wand presets.
- `phone-controls.js` contains pure touch-stick and aim-assist scoring helpers.
- `network-validation.js` contains reusable packet bounds, player/state/cast checks and a rate-limit primitive. These checks are defensive client-side validation, not server authority.
- `room-rules.js` centralizes room-code normalization, supported game modes, team-size requirements and allowed arena IDs.

Run `npm test` for the app's Vitest suite and native Node tests, or `npm run test:game` to run the extracted game-module tests alone. The modules are served as static JavaScript alongside the game, so no bundler runtime is required by `public/game.html`.

### Defensive spell synchronization (step 4)

Defenses remain geometric and distinct: flame ring, glacier shell, storm dome, venom veil, stone bulwark, gale rings, tidal sphere, voidstep rings, radiant panel and mirror panes. Realtime events carry a validated defense key, per-player sequence, cast yaw and bounded lifetime; lifetime updates and end events remove remote visuals promptly. The replicated meshes are visual representations. Collision still runs on the defending player's client, so this is not server-authoritative combat and must be followed by hit-confirmation work and a server decision for competitive play.

### Homing projectiles (step 6)

When a cast includes the validated `h: 1` modifier, the sender's local projectile steers toward the nearest live opposing avatar using the latest replicated avatar positions. Each receiving client independently resolves the opposing target from its own roster/state and steers its visual projectile copy toward that avatar (in a 1v1 match, this is the receiving player). Target IDs and steering instructions are not accepted from cast packets. Team matches exclude friendly avatars; free-for-all treats every other player as an opponent. The steering helper preserves projectile speed and keeps a valid target locked while it remains available.

This update changes projectile guidance only. Player-hit confirmation, damage replication, status effects and authoritative kill handling remain subsequent work; the current client-hosted realtime design is still not authoritative combat.

## Online play

Online matches run peer-to-peer and are hardened against simple forgery; read [HARDENING.md](HARDENING.md) for what is and is not enforced.
