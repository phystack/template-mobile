# CLAUDE.md — template-mobile

Starter template for PhyStack **WEB** apps: a Vite + React 18 + TypeScript
mobile-PWA bundle opened in a visitor's browser, connected to the platform
through `@phystack/hub-client` (Web twins are structurally Screen twins; the
2026-03 API renamed the legacy `mobile` type to `WEB`). Scaffolded by
`phy app init <name> --type web`.

**Lockstep rule:** this template is a copy of
[template-screen-react](https://github.com/phystack/template-screen-react)
with exactly three differences — `application-type: web`, the PWA layer
(`public/manifest.webmanifest`, theme-color + apple-touch-icon metas,
`logo192/512` icons), and naming. If you change something here, apply it
there too (and vice versa).

## Commands (bun-only — no npm/yarn scripts)

| Command | What it runs |
|---|---|
| `bun install` | Install dependencies |
| `bun run dev` | `phy-simulator run . --dev-command 'bun run start'` — local simulated device + vite dev server, auto-opens the browser |
| `bun run start` | Schemas + `vite` only (expects a running simulator) |
| `bun run build` | `tsc -b` + `vite build` + schemas + `scripts/post-build.js` |
| `bun run schema` | `scripts/build-schema.js` + `scripts/build-analytics-schema.js` → `build/` |
| `bun run pub` | `bun run build && phy app build create $npm_package_name --dir . --publish` |
| `bun run lint` / `format` / `format:check` | eslint / prettier |

## Dev loop

- `bun run dev` needs the standalone simulator installed once:
  `npm i -g @phystack/device-simulator` (provides the `phy-simulator`
  binary). It boots a simulated device on `:55000`, starts vite, and opens
  the browser at `/#instanceId=<twinId>` — hub-client reads the instance id
  from the URL hash.
- The hub connection is a per-window singleton; `connectPhyClient()` is
  called once from `src/App.tsx` and signals/analytics reuse the same
  socket. Do not open a second connection.
- Settings come from the twin's desired properties;
  `scripts/init-settings.js` seeds local defaults from the schema.

## Schema pipeline

`src/schema.ts` → `scripts/build-schema.js` → `build/schema.json`
(validation) + `build/meta-schema.json` (Console UI hints).
`src/analytics-schema.ts` → `build/analytics-schema.json`.
`scripts/post-build.js` finalizes the bundle layout after `vite build`.

## Publish flow (new `phy` CLI grammar)

```bash
phy login
phy app create <name> --type web   # register in your tenant (once)
bun run pub                        # build, submit + publish (no container image)
```

Web builds ship no container, so there is no `--push` and no registry login.
The legacy `@phystack/cli` (Node) does not work with this template — use the
Rust `phy` CLI only.

## Layout

| Path | Purpose |
|---|---|
| `src/App.tsx` | UI + hub-client connection, settings, twin messaging, signals |
| `src/schema.ts` | Installation-settings schema source |
| `src/analytics-schema.ts` | Analytics events this app emits |
| `public/manifest.webmanifest` | PWA manifest (installable on visitors' phones) |
| `scripts/` | Schema build, local settings seed, post-build fixup |
| `vite.config.ts` | Dev server (port 3000) + simulator hand-off via `#instanceId` |

## Gotchas

- The vite dev server defaults to port 3000, shared with screen-app
  scaffolds — only one can serve at a time, and stale browser tabs from
  sibling apps stay connected to the simulator (they show up as extra
  "unknown" instances in its log).
- `application-type` in package.json must stay `web`; the package.json
  `name` is the app name used by `pub` (`$npm_package_name`).
- Keep the PWA metadata (`manifest.webmanifest`, icons, theme-color) in sync
  with the app's branding — web apps live on visitors' home screens.
