# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

A small Express (CommonJS, Node 22) REST API for users, backed by an in-memory store.

## Commands

- `npm run dev` — start the API with `node --watch` on http://localhost:3000 (`PORT` overrides)
- `npm test` — run all tests with the built-in `node:test` runner
- `node --test tests/users.test.js` — run one test file; add `--test-name-pattern="404"` to run a single test
- `npm run lint` — ESLint (`eslint:recommended`)

CI (`.github/workflows/ci.yml`) runs `npm run lint` then `npm test` on Node 22; both must pass.

## Conventions

- Use CommonJS (`require` / `module.exports`), not ES modules — ESLint is configured with `sourceType: "script"`.
- Write tests with `node:test` + `node:assert` and `supertest` against the exported `app`, not Jest/Mocha and not a live server.
- Route handlers access data only through functions exported from `db/store.js`; never keep data arrays in route files.
- Error responses use `res.status(<code>).json({ error: "<message>" })`; return early on validation failures (400) and missing records (404).
- Parse `:id` route params with `Number(...)` — the store compares ids with `===`.
- Never read or commit `.env`; document new config keys in `.env.example` instead.

## Architecture

- `server.js` builds the Express app, mounts one router per resource (`/users`, `/health`), and only calls `listen()` when run directly (`require.main === module`). It exports `app` so tests can import it without opening a port.
- `routes/` — one `express.Router()` file per resource; add a new resource by creating a router file and mounting it in `server.js`.
- `db/store.js` — module-level in-memory arrays standing in for a database. State resets on restart but is **shared across all tests in a process**, so tests must not assume a fixed user count or ids beyond the two seeded users.
