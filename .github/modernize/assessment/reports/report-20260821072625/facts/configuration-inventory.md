# Configuration & Externalized Settings Inventory

The application has a minimal configuration surface: one Dockerfile and one `package.json` define all configuration — there are no `.env` files, config server references, or secrets stores.

## Configuration Sources

| Source | Type | Path/Location | Notes |
|--------|------|--------------|-------|
| `package.json` | npm package manifest | `/package.json` | Declares dependencies, scripts, metadata |
| `Dockerfile` | Container definition | `/Dockerfile` | Sets working directory, exposes port 3000, defines entrypoint |

No `.env`, `.env.local`, `.env.production`, `config/*.js`, or any other environment-specific configuration files are present.

## Build Profiles

| Profile | Activation | Purpose | Key Dependencies/Plugins |
|---------|-----------|---------|--------------------------|
| default (npm start) | `npm start` | Runs the application with Node.js | All `dependencies` in `package.json` |

No webpack, vite, or multi-environment build configuration exists. There is a single `npm start` script (`node index.js`) and a stub `test` script.

## Runtime Profiles

| Profile | Activation Method | Config Files | Key Overrides |
|---------|------------------|-------------|---------------|
| default | N/A (no profile system) | None | None |

No `NODE_ENV`-based branching, `.env.development`, or `.env.production` files are present. The application behaves identically regardless of environment.

## Properties Inventory

| Property Key | Default | Profiles | Source |
|-------------|---------|---------|--------|
| `PORT` (implicit) | 3000 (hardcoded in `index.js`) | All | Hardcoded in source |
| Wikipedia API URL | `https://en.wikipedia.org/w/api.php` | All | Hardcoded in source |

No externalized properties, environment variable references (`process.env.*`), or configuration files are used. All settings are hardcoded in `index.js`.

## Startup Parameters & Resource Requirements

| Service | Runtime Options | Memory | Instance Count |
|---------|----------------|--------|----------------|
| simple-nodejs-app | None specified | Not specified | 1 (single process) |

The Docker entrypoint `ENTRYPOINT start npm` appears to be a misconfigured entrypoint (should likely be `CMD ["npm", "start"]` or `ENTRYPOINT ["node", "index.js"]`). No JVM/Node heap settings, CPU limits, or memory limits are configured.

## Startup Dependency Chain

No startup dependencies. The application connects to the Wikipedia API on-demand per request and has no services it must wait for at startup (no database, no config server, no service discovery).

## Secrets & Sensitive Configuration

| Secret Reference | Type | Storage |
|-----------------|------|---------|
| None | N/A | N/A |

No API keys, database passwords, or other secrets are used or configured. The application calls the Wikipedia public API without authentication. No secrets management solution (HashiCorp Vault, AWS Secrets Manager, Azure KeyVault) is present.

### Secrets Provisioning Workflow

No secrets provisioning workflow exists. The application requires no credentials to operate.

## Feature Flags

No feature flags or conditional configuration are implemented. There are no `ConditionalOnProperty`, LaunchDarkly, Unleash, or custom toggle mechanisms.

## Framework & Runtime Versions

| Component | Version | Source |
|-----------|---------|--------|
| Node.js | 22.23.2 (runtime); `node` (latest) in Docker | Environment / Dockerfile |
| Express | ^4.17.3 | package.json |
| EJS | ^3.1.10 | package.json |
| request | ^2.88.2 | package.json |
| node-fetch | ^2.6.7 | package.json |
| nodemon | ^2.0.20 | package.json |
| wiki-infobox-parser | ^0.1.11 | package.json |
| Docker base image | `node` (untagged — pulls latest) | Dockerfile |
| npm | 10.9.8 | Environment |
