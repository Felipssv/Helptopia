# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Commands

```bash
# Install dependencies
npm install

# Start server (production)
npm start

# Start server with auto-reload (development)
npm run dev
```

The app runs at `http://localhost:3000`. There are no tests configured (`npm test` exits with an error).

## Architecture

**Helptopia** is an Express.js web app serving as a beginner's guide for the game *Heartopia*. It provides authentication, a game guide, an interactive map, and promotional codes.

**Request flow:**
1. `index.js` — boots Express, registers session middleware, exposes `res.locals.usuario` for all EJS templates, and mounts both route files.
2. `db.js` — exports a single `pg.Pool` instance connected to PostgreSQL (Supabase) via `DATABASE_URL`.
3. `routes/authRoutes.js` — handles `POST /login`, `POST /register`, `GET /logout`.
4. `routes/pageRoutes.js` — handles all page GETs; `/codigos` is the only route that queries the DB directly (no model layer).
5. `models/usuarioModel.js` — the only model file; all user queries live here (`criarUsuario`, `buscarPorNome`, `buscarPorEmail`).
6. `views/*.ejs` + `views/partials/` — EJS templates; partials (`header.ejs`, `navbar.ejs`, `footer.ejs`) are included in every page.

**Session:** `req.session.usuario` holds `{ id, nome, email }` when logged in. It is automatically injected into every template as `usuario` via middleware in `index.js`.

## Key Conventions

**Database:**
- Use the native `pg` driver (already imported from `db.js`). Do **not** introduce ORMs.
- All SQL must use parameterized queries (`$1`, `$2`, …) — never string-concatenated SQL.
- New DB logic for users goes in `models/usuarioModel.js`. Other entities can get their own model files.
- `codigos` table is queried directly in `pageRoutes.js` (no model); keep simple queries there, extract to a model if they grow.

**Auth:**
- Passwords must be hashed with `bcrypt` (cost factor 10) before storage. Never store plain text.
- Login accepts username (`nome`), not email. Both `buscarPorNome` and `buscarPorEmail` exist for register duplicate-checking.

**EJS Templates:**
- Always use `<%= %>` (escaped) for user-supplied or DB-sourced content — never `<%-` unless the HTML comes from trusted internal code.
- Each template receives a `pagina` local (e.g., `"home"`, `"guia"`) used by the navbar to highlight the active link.
- Error messages are passed as the `erro` local and displayed inline on the same page (`res.render(..., { erro: "..." })`).

**Routing:**
- Auth mutations (`POST /login`, `POST /register`) → `authRoutes.js`
- Page GETs → `pageRoutes.js`

**Styling:**
- Vanilla CSS only in `public/css/style.css`. No CSS frameworks.
- UI text must be in Portuguese (Brazil).

## Frontend & Accessibility (views/ and public/)

Any change to `views/` or `public/` must follow WCAG 2.1 AA. Run `/wcag` to audit.

**Mandatory checklist before committing frontend changes:**
1. Every `<img>` has a descriptive `alt` (or `alt=""` if decorative)?
2. All links and buttons have visible focus (`:focus` / `:focus-visible`)?
3. Each `<input>` has a matching `<label for="id">`?
4. Single `<h1>` per page; heading hierarchy (`h1→h2→h3`) has no gaps?
5. Text contrast ≥ 4.5:1 against background?
6. Error/required-field indicators use text — not color alone?
7. Semantic tags (`<main>`, `<nav>`, `<header>`, `<footer>`) are present?
8. Layout holds at 200% zoom?

Use `<%= %>` (HTML-escaped) for all user-supplied or DB-sourced values in EJS — never `<%-` unless the HTML originates from internal trusted code.

## Security

- SQL: parameterized queries (`$1`, `$2`) are mandatory everywhere — no string concatenation in SQL.
- Passwords: `bcrypt.hash(senha, 10)` before INSERT; `bcrypt.compare` on login. Never store plaintext.
- Session secret must come from `SESSION_SECRET` in `.env`, not hardcoded.
- Do not leak stack traces to the client — log with `console.error`, respond with a user-friendly message in Portuguese.
- Do not introduce new heavy dependencies without explicit request; keep the `pg` native driver.

## Custom Commands

| Command | Purpose |
|---------|---------|
| `/dev-environment` | Step-by-step local setup (PostgreSQL, `.env`, table creation) |
| `/prd` | Generate a Product Requirements Document for a new feature |
| `/wcag` | WCAG 2.1 AA accessibility audit for EJS views or CSS |

## Environment

Required `.env` variables:

```
DATABASE_URL=postgresql://...   # Supabase or local PostgreSQL connection string
SESSION_SECRET=...              # Random string; never hardcode in index.js
```
