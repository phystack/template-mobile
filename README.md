# template-mobile

Starter template for PhyStack **WEB** apps — mobile-PWA front-end bundles
opened in a visitor's browser (the 2026-03 API renamed the legacy `mobile`
type to `WEB`). Scaffolded by the PhyStack CLI (`phy app init --type web`)
or usable directly.

## Identical to the screen template, by design

Web apps are architecturally the same as screen apps on today's platform: a
front-end bundle connecting through `@phystack/hub-client`, with settings
delivered via the app twin and the instance identified by `#instanceId` in
the URL. This template is therefore a copy of
[template-screen-react](https://github.com/phystack/template-screen-react)
(Vite + React + hub-client), with exactly these differences:

| Difference | Value here |
|---|---|
| `application-type` in package.json | `web` (screen template: `screen`) |
| Name / title / this README | mobile wording |

Nothing else differs — dependencies, scripts, schema pipeline, vite config
and source layout are identical. If you find yourself changing one template,
change both.

## Getting started

```bash
# Scaffold via the PhyStack CLI
phy app init my-web-app --type web

# Or work directly from this template
bun install
bun run build
```

## Flow

```bash
bun run dev        # local development
bun run build      # typecheck + vite build + schemas into build/
phy app create my-web-app --type web    # register (once)
bun run pub        # build + submit + publish (no container image for web)
```
