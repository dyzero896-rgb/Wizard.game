<!-- LOVABLE:BEGIN -->
> [!IMPORTANT]
> This project is connected to [Lovable](https://lovable.dev). Avoid rewriting
> published git history — force pushing, or rebasing/amending/squashing commits
> that are already pushed — as it rewrites history on Lovable's side and the
> user will likely lose their project history.
>
> Commits you push to the connected branch sync back to Lovable and show up in
> the editor, so keep the branch in a working state.
<!-- LOVABLE:END -->

- The game entry point remains public/game.html (standalone Three.js); src/routes/index.tsx only shows it full-screen in a frame. Pure helpers and data should be extracted into public/game-modules/*.js and imported by game.html. Keep changes behavior-preserving; do not bundle the standalone game into React components.
- Online play uses Lovable Cloud realtime broadcast+presence (no tables); each player decides hits on themselves so their shields work. Why: there is no game server.
- Phone input stays in the standalone game with pointer ownership, reset-on-interruption, and bounded aim assistance. Why: simultaneous movement, looking, and firing must work without stuck inputs or changing desktop combat.
