# AGENTS.md

## Cursor Cloud specific instructions

This is a full-stack real-time chat sample: an Express + Socket.IO backend and an Angular 17 (SSR) frontend.

### Services

| Service  | Dir         | Dev command     | Port | Notes                                                        |
| -------- | ----------- | --------------- | ---- | ----------------------------------------------------------- |
| backend  | `backend/`  | `npm run dev`   | 3000 | Express + Socket.IO, hot-reload via nodemon. `npm run prod` runs without reload. |
| frontend | `frontend/` | `npm start`     | 4200 | `ng serve --host 0.0.0.0 --port 4200`. Connects to Socket.IO at `http://localhost:3000` (see `src/environments/environment.ts`). |

Run both for the chat to work end-to-end. The frontend's chat client talks to the backend over Socket.IO on port 3000, so the backend must be running for messages to send/echo.

### Non-obvious caveats

- Run natively for development (`npm run dev` / `npm start`); the README's `docker compose -f compose.dev.yaml up --build` also works but is not required and is heavier.
- `ng serve`/`ng build` will prompt interactively to share analytics on first run, which blocks the dev server. Angular analytics has already been disabled globally (`ng analytics disable --global`); if you hit the prompt in a fresh environment, run that command or answer `N`.
- `ng build` prints "unused file" TypeScript warnings for `server.ts`, `app.config*.ts`, `app.routes.ts`, `main.server.ts`. These are harmless and the build still succeeds.
- There are no real backend tests (`backend` `npm test` is a placeholder that exits 1). Frontend tests use Karma + headless Chrome (`npm test`).
