# 12. Tooling and React with Vite

Previous: [11. TypeScript](11-typescript-for-front-end.md) | Next: [13. React rendering](13-react-rendering-and-components.md)

## What each tool owns

React is the UI library. Vite supplies development/build tooling; the default React template is a useful SPA starting point. A router, server-state library, auth integration, and deployment behavior remain application decisions. Vite supports SSR infrastructure too, but a plain SPA template is not automatically a full-stack framework.

Create React App is deprecated; know it for legacy interviews rather than choosing it as the default new setup. This is documented in the [React team's announcement](https://react.dev/blog/2025/02/14/sunsetting-create-react-app).

## Practice setup, PowerShell

Run these later when you are ready to build. They create a new practice subdirectory, not a replacement for the study notes.

```powershell
node --version
npm --version
npm create vite@latest practice-vite -- --template react-ts
Set-Location practice-vite
npm install
npm run dev
```

Use a supported Node version that satisfies the selected template's engines. Inspect generated scripts before relying on them. The [Vite getting-started guide](https://vite.dev/guide/) is the setup reference. Commit the lockfile in a real practice repository and use the package manager it belongs to. `npm ci` expects a consistent package lock and performs a clean reproducible install.

## Development versus production

During development, Vite serves a module graph and supports hot updates. Production emits optimized assets. Current Vite documentation describes Rolldown-based bundling; older projects may use a different pipeline. Do not repeat the outdated claim that every Vite version uses the same bundler. Study the installed major and plugins when comparing systems.

HMR replaces affected modules; React Fast Refresh can preserve suitable component state. Neither is proof a production build behaves identically. Tree shaking relies on analyzable module structure and side-effect information. Code splitting changes when code loads, not whether the code has a runtime cost when used.

TypeScript transformation and TypeScript checking are separate jobs. Vite can transform a `.ts` file without proving its types are valid; use the project's type-check command as well as a build. See [Vite features](https://vite.dev/guide/features).

## Environment and deployment

```ts
const apiBase = import.meta.env.VITE_API_BASE_URL;
```

Client-exposed environment values are public. Vite replaces these values at build time, and exposed custom values are generally strings, so parse them deliberately. Never put a private API key into a `VITE_` variable. Mode and `NODE_ENV` are related but distinct; use the [environment guide](https://vite.dev/guide/env-and-mode) to verify behavior.

A local API proxy simplifies development but does not configure production CORS or authentication. Production needs an actual API origin or reverse proxy. Browser-history routes need a hosting fallback so reloading `/houses/123` serves the SPA entry instead of a host 404. Preserve real asset/API 404s rather than rewriting every missing file into HTML.

Use content-hashed assets with long immutable cache lifetimes and an HTML policy that allows deployments to become visible. Retain needed old chunks during rollout so users with older HTML do not hit missing dynamic imports. `vite preview` is a local build preview, not the production server strategy.

## Toolchain skills and exercise

Understand package scripts, dependencies versus dev dependencies, semver ranges, lockfiles, ESM versus CommonJS, source maps, linting, formatting, module aliases, asset imports, and browser support targets. A TypeScript path alias must also be understood by the bundler/runtime used in that app.

Scaffold the app, add one environment-driven public API base, lazy-load a page, run lint/type-check/build scripts that actually exist, and preview the build. Test direct navigation on a production-like static host. Write down the install commands and versions in your practice project's notes.

## Questions and answer cues

**Why Vite instead of Next.js?** Choose based on rendering, server requirements, deployment, and team needs, not dev-server speed alone. **Why does a deep link fail only after deployment?** Check history fallback and base path. **Why did a secret appear in the bundle?** Client environment substitution is exposure, not secret storage.

Exit gate: explain the complete path from source to deployed assets and identify which responsibilities Vite leaves to your application.
