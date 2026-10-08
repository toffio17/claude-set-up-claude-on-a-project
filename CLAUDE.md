# CLAUDE.md

Express 4 REST API (users + health check) backed by an in-memory store.

## Commands
- `npm run dev`: start the API with auto-reload on http://localhost:3000 (override with `PORT`)
- `npm test`: run the tests (Node's built-in `node --test` + supertest)
- `npm run lint`: ESLint (`eslint:recommended`)
- CI runs `npm run lint` then `npm test` on Node 22. Run both before committing.

## Conventions
- Use CommonJS (`require` / `module.exports`), not ES modules (`import`/`export`).
- Write tests with `node:test` + `node:assert` + `supertest`, not Jest or Mocha. Put them in `tests/*.test.js` and import `app` from `server.js`. Never call `app.listen()` in tests.
- Routes never touch data directly. All data access goes through functions exported from `db/store.js`; add a new store function rather than reaching into the data.
- Errors return JSON `{ "error": "<message>" }` with the right status: 400 for invalid input, 404 for not found. A successful create returns 201.
- Route params are strings: convert ids with `Number(req.params.id)` before looking them up.
- Don't add new npm dependencies without asking first.

## Architecture
- `server.js`: creates the Express app, mounts one router per resource and exports `app`. It only calls `listen()` when run directly, so tests can import it.
- `routes/<resource>.js`: one `express.Router()` per resource, mounted at `/<resource>` in `server.js`.
- `db/store.js`: in-memory data standing in for a database. Data resets on every restart.
