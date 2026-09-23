# Changelog

## tidal-tools 0.3.2 — 2026-09-23

- Explicit CLI install matrix: `curl https://get.tidal.sh/unix | bash` plus offline/direct binary URLs
- Prefer https://get.tidal.sh/AGENTS.md when available
- Public docs scrub for release-mirror wording

## tidal-tools 0.3.1 — 2026-09-23

Initial public release of the **tidal-tools** agent skill under `tidalmigrations/agent-skills`.

- Install / auth (`tidal doctor`, `tidal login`, `tidal ping`; explore via `tidal help`)
- Optional custom fields (`POST /api/v1/fields`)
- Create Accelerator application (`POST /api/v1/apps/import` / `tidal request`)
- `tidal analyze code` → Source Code Readiness
- Portfolio business context for AWS Transform handoffs

License: Apache-2.0.
