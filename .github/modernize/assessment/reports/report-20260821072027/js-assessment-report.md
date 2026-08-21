# JavaScript Dependency Assessment Report

Generated: 2026-08-21T07:20:27Z

## Summary

Running `npm-check-updates` against `package.json` revealed the following available updates:

## Available Updates

### Major — Potentially breaking API changes

| Package | Current | Latest |
|---------|---------|--------|
| `ejs` | `^3.1.10` | `^6.0.1` |
| `express` | `^4.17.3` | `^5.2.1` |
| `node-fetch` | `^2.6.7` | `^3.3.2` |
| `nodemon` | `^2.0.20` | `^3.1.14` |

## Notes

- `request` and `wiki-infobox-parser` are at their latest versions.
- All major updates may contain breaking API changes; review changelogs before upgrading.
- `express` v5 is now stable and includes improved error handling and async support.
- `node-fetch` v3 is ESM-only; migrating requires changing import syntax.

## Recommendations

1. Upgrade `nodemon` to v3 — low risk, mostly internal changes.
2. Upgrade `express` to v5 — moderate risk, review migration guide.
3. Upgrade `node-fetch` to v3 — requires ESM migration.
4. Upgrade `ejs` to v6 — review template syntax changes.
