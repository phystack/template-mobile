# template-mobile

Starter template for Phystack mobile edge modules, scaffolded by `@phystack/cli`.

## Overview

This template provides a minimal React application that runs as a `mobileapp` module on a Phystack edge device. It demonstrates settings integration, real-time messaging (publish/subscribe), and connection status handling. The scaffolded app is intended as a starting point -- replace the sample ping/pong logic with your own functionality.

## Tech Stack

| Layer | Technology |
|-------|------------|
| Language | TypeScript |
| UI | React 16, styled-components |
| Messaging | `@ombori/ga-messaging` (WebSocket) |
| Settings | `@ombori/ga-settings` |
| Build | react-scripts (CRA) |
| Schema | `@ombori/ts-schema` |

## Prerequisites

- Node.js 12+
- Yarn

## Getting Started

```bash
cd edge/template-mobile
yarn install
yarn bootstrap        # copies default settings into src/settings/
yarn start            # starts dev server with local WebSocket messaging
```

The dev server connects to a local messaging endpoint at `ws://localhost:8088`.

## Project Structure

```
src/
  App.tsx              # Main component with messaging example
  schema.ts            # Settings schema (generates JSON Schema at build)
  index.tsx            # React entry point
  index.css            # Global styles
default.settings.json  # Default settings for local development
meta/                  # App store screenshots
public/                # Static assets and HTML shell
DESCRIPTION.md         # App store listing (markdown template)
package.json
```

## Settings Schema

Settings are defined in `src/schema.ts` and compiled to JSON Schema during build. The default schema includes two example fields:

| Field | Type | Default | Description |
|-------|------|---------|-------------|
| `productName` | string | `"My Example Product"` | Display name |
| `productPrice` | string | `"99 USD"` | Formatted price |

Replace these with your own fields. Local development reads from `default.settings.json`.

## Usage

This template is consumed by `@phystack/cli` to scaffold a new mobile edge module:

```bash
npx @phystack/cli create --template mobile my-app
```

The CLI clones this repo, replaces placeholder values in `DESCRIPTION.md` and `package.json`, and sets up the new project directory. After scaffolding, install dependencies and start developing:

```bash
cd my-app
yarn install
yarn start
```

### Build and Publish

```bash
yarn build    # production build + schema generation + app packaging
yarn pub      # publish to the Phystack app registry
```

## Related Documentation

- [DESCRIPTION.md](./DESCRIPTION.md) -- app store listing template
- [Phystack CLI documentation](https://github.com/phystack/cli)
