# tidal-tools — reference

## Purpose

Portable skill for agents (Cursor, Claude Code, Codex, Kiro, and others) that need to drive **Tidal Tools** with **Tidal Accelerator** across assessment and modernization workflows:

- **Install and auth** — `curl https://get.tidal.sh/unix | bash` (or offline binaries on get.tidal.sh); prefer https://get.tidal.sh/AGENTS.md when present; then `tidal doctor`, `tidal login` / `tidal ping`
- **Discovery** — `tidal discover` (domains/URLs; see https://guides.tidal.cloud/discover.html) and related collection paths documented in the Tidal Tools guides
- **Data import and sync** — bring apps/servers/databases (and related inventory) into Accelerator via documented import APIs (`tidal request`, https://guides.tidal.cloud/import-apps.html) and `tidal sync` (apps, servers, dbs, nmap, vsphere; https://guides.tidal.cloud/sync-servers.html); create custom fields when assessment metadata must stick
- **Source analysis** — `tidal analyze code` (and related analyze flows) → Source Code Readiness in Accelerator
- **Portfolio business context** — fetch application (and related) portfolio data from Accelerator with `tidal export` / `tidal request` so agents can ground modernization and transformation work—especially handoffs into **AWS Transform**—in real inventory, ownership, and readiness signals rather than invented context
- **CLI surface of record** — run `tidal help` to list commands, then `tidal <command> --help` (and nested help) for discovery methods, sync/import/export/analyze actions, and flags. Prefer live help output over memorized schemas; do not invent subcommands.

**Audience:** AWS SI partners and consultants using Accelerator for discovery and assessment before modernization, migration, and transformation.

## Workflow (summary)

Happy path for source-code readiness:

1. Install CLI via `curl https://get.tidal.sh/unix | bash` (or platform binary URLs on get.tidal.sh) → `tidal doctor` / Docker
2. `tidal login` → `tidal ping`
3. Optional: create custom fields via `POST /api/v1/fields` (see `references/custom-fields.md`)
4. Create app: `tidal request -X POST /api/v1/apps/import` with `{"apps":[…]}` (https://guides.tidal.cloud/import-apps.html) or reuse existing app id
5. `tidal analyze code --app-id N` (or offline → upload)
6. Accelerator UI: Source Code Readiness on the application

For other discovery methods and actions, start with `tidal help`.

## Dependencies

- Tidal Tools CLI + Docker
- Accelerator workspace credentials
- Optional: AMA demo-data zip (equinox-project default, nop-commerce fuller)

## Guides consulted

- https://get.tidal.sh
- https://guides.tidal.cloud/tidal-tools.html
- https://guides.tidal.cloud/discover.html
- https://guides.tidal.cloud/sync-servers.html
- https://guides.tidal.cloud/analyze-source-code.html
- https://guides.tidal.cloud/modernization-assessment.html
- https://guides.tidal.cloud/import-apps.html
- Tenant API docs: `https://[subdomain].tidal.cloud/api_docs`
- API base: `https://[subdomain].tidal.cloud/api/v1/`
- Live CLI: `tidal help`, `tidal <command> --help`

## Example prompts

- "Install Tidal Tools, create an app for this equinox sample, run analyze code, and show me where results land."
- "Use tidal-tools on ./equinox-project."
- "Create a custom field for assessment notes, import an app with that field set, then run tidal analyze code."
- "Discover or sync this estate into Accelerator, then list apps I can use for an AWS Transform prep."
- "Export or request application business context from Accelerator for an AWS Transform handoff."
- "Run `tidal help` and propose which discover/sync path fits this estate."
- "Prepare Accelerator application context from this analysis for an AWS Transform handoff."
