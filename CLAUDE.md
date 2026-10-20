# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

Express REST API (users + health check) backed by an in-memory store. It's the starter project for a Claude Code course.

## Commands

- `npm run dev`: start the API with auto-reload on http://localhost:3000
- `npm test`: run all tests (`node --test`). Use `node --test tests/users.test.js` to run one file.
- `npm run lint`: run ESLint. CI runs lint and then tests, and both must pass.

## Conventions

- Use CommonJS (`require` / `module.exports`), not ES modules.
- Write tests with `node:test` + `node:assert` and `supertest` against the exported `app`, not Jest or Mocha. Never call `listen()` in tests.
- Read and write data only through the functions in `db/store.js`, never directly from a route.
- Return errors as `{ error: "<message>" }` with 400 (validation) or 404 (not found). Return 201 on create.

## Architecture

- `server.js` is the entry point. It builds the app, mounts the routers, exports `app`, and only listens when run directly.
- `routes/` has one `express.Router()` file per resource. A new resource means a new file plus an `app.use()` line in `server.js`.
- `db/store.js` holds in-memory data that resets on restart. That data is shared across tests in one run, so don't assume exact counts.
