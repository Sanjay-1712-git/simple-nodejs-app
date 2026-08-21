# Configuration & Externalized Settings Inventory

This application has a **minimal configuration footprint**: all settings are either hardcoded in `index.js` or defined in `package.json`. There are no `.env` files, no runtime profiles, and no secret management tooling.

## Configuration Sources

| Source | Type | Path/Location | Notes |
|--------|------|--------------|-------|
| `package.json` | npm manifest | `/package.json` | Declares dependencies, scripts (`start`, `test`), and package metadata |
| `Dockerfile` | Container config | `/Dockerfile` | Defines runtime image; contains a bug (`ENTRYPOINT start npm` is invalid) |
| `.gitignore` | VCS config | `/.gitignore` | Excludes `node_modules` only |
| Inline code | Hardcoded | `/index.js` | Port `3000` and Wikipedia API URL are hardcoded directly in source |

No `.env` files, Spring Cloud Config, Key Vault, Vault, Consul, or any other external configuration source is used.

## Build Profiles

| Profile | Activation | Purpose | Key Dependencies/Plugins |
|---------|-----------|---------|--------------------------|
| (none) | N/A | No build profiles are defined | `npm install` installs all deps; no dev/prod split |

The project has no webpack, vite, esbuild, or any bundler configuration. There is no separate dev/prod build pipeline. `npm start` runs `node index.js` directly.

## Runtime Profiles

| Profile | Activation Method | Config Files | Key Overrides |
|---------|-----------------|-------------|---------------|
| (none) | N/A | None | No environment-specific configuration exists |

No `NODE_ENV` branching, no `.env.development` / `.env.production` files, and no profile-conditional logic are present in the codebase.

## Properties Inventory

| Property Key | Default/Value | Profiles | Source |
|-------------|--------------|---------|--------|
| HTTP port | `3000` | All | Hardcoded in `index.js` (`app.listen(3000, ...)`) |
| Wikipedia API base URL | `https://en.wikipedia.org/w/api.php` | All | Hardcoded in `index.js` |
| View engine | `ejs` | All | Hardcoded in `index.js` (`app.set("view engine", 'ejs')`) |
| Start script | `node index.js` | All | `package.json` `scripts.start` |

No properties are sourced from environment variables. All configuration is embedded in source code.

## Startup Parameters & Resource Requirements

| Service | Runtime Options | Memory | Instance Count |
|---------|----------------|--------|---------------|
| simple-nodejs-app | None specified | Not configured | 1 (single process) |

The Dockerfile does not set memory limits or CPU allocations. The `ENTRYPOINT` instruction in the Dockerfile is malformed (`ENTRYPOINT start npm` — should be `CMD ["npm", "start"]` or `ENTRYPOINT ["node", "index.js"]`), meaning the containerised version will fail to start.

## Startup Dependency Chain

There are no inter-service dependencies. The application starts a single Node.js process, binds to port 3000, and is immediately ready to serve requests. No health checks, readiness probes, or wait mechanisms are configured.

## Secrets & Sensitive Configuration

| Secret Reference | Type | Storage |
|-----------------|------|---------|
| Wikipedia API | Public API (no key required) | N/A — no credentials needed |

No secrets, API keys, passwords, or credentials are present in the codebase. The Wikipedia OpenSearch API used by this application does not require authentication.

### Secrets Provisioning Workflow

No secrets provisioning workflow exists. The application requires no credentials to function. If future integrations require API keys, it is recommended to use environment variables injected at runtime (e.g., via a `.env` file with `dotenv`, or container environment variables) rather than hardcoding values in source.

## Feature Flags

| Flag Name | Default | Controlled By |
|-----------|---------|--------------|
| (none) | N/A | No feature flags are configured |

## Framework & Runtime Versions

| Component | Version | Source |
|-----------|---------|--------|
| Node.js | 22.x (runtime), unspecified in Dockerfile (`FROM node`) | Runtime environment / `Dockerfile` |
| Express | ^4.17.3 | `package.json` |
| EJS | ^3.1.10 | `package.json` |
| request | ^2.88.2 | `package.json` |
| node-fetch | ^2.6.7 | `package.json` |
| nodemon | ^2.0.20 | `package.json` |
| wiki-infobox-parser | ^0.1.11 | `package.json` |
| Docker base image | `node` (latest, untagged) | `Dockerfile` |
| npm | 10.9.x | Runtime environment |

> Note: The Dockerfile uses `FROM node` without a version tag, meaning the image used during builds is non-deterministic and can change unexpectedly. Pinning to a specific version (e.g., `FROM node:22-alpine`) is strongly recommended.
