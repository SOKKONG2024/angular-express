# AGENTS.md

## Cursor Cloud specific instructions

This repo is a Defang sample: a real-time chat app with an Angular 17 frontend
(`frontend/`) and a Node.js/Express + Socket.IO backend (`backend/`). No
database or other external services are required — backend state is in-memory.

Dependencies are installed by the startup update script (`npm install` in both
`backend/` and `frontend/`). The notes below cover only non-obvious run/test
caveats.

### Running the two services (development)

Run the services directly with npm (Docker is not required for local dev):

- Backend: `npm run dev` in `backend/` → Express + Socket.IO on port `3000`
  (nodemon hot-reload; `PORT` env overridable).
- Frontend: `npm start` in `frontend/` → `ng serve` on port `4200`. First
  compile takes ~30–60s.

The frontend connects to the backend at `http://localhost:3000` (hardcoded in
`frontend/src/environments/environment.ts`), so both must run for the chat to
work. The README's `docker compose -f compose.dev.yaml up --build` also works
but is slower.

### Angular CLI interactive prompt (gotcha)

On a fresh VM the Angular CLI (`ng serve`, `ng build`, `ng test`) prompts once
for anonymous analytics and **blocks/hangs waiting for TTY input**. Run Angular
commands non-interactively by disabling analytics, e.g.:

```bash
NG_CLI_ANALYTICS=false npm start
```

### Tests / lint (pre-existing repo state)

- Backend has no real tests (`npm test` is a placeholder that exits 1).
- No lint target is configured for either service (Angular 17 ships without one;
  `angular.json` has no `lint` architect target).
- `npm test` in `frontend/` (`ng test`, Karma) currently **fails out of the
  box** because `angular.json`'s `test` target references `src/test.ts`, which
  does not exist in the repo. This is a pre-existing repo misconfiguration, not
  an environment problem. Chrome is available at
  `/usr/bin/google-chrome-stable`; run headless with
  `NG_CLI_ANALYTICS=false CHROME_BIN=/usr/bin/google-chrome-stable npm test -- --watch=false --browsers=ChromeHeadless`
  once the test config is fixed.
- `NG_CLI_ANALYTICS=false npm run build` in `frontend/` works (warnings only).
