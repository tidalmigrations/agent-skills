# Tidal Agent Skills

Public [Agent Skills](https://agentskills.io)-compatible skills for **Tidal Tools**, aimed at **AWS SI partners** and consultants who use Tidal Accelerator for discovery and assessment before modernization, migration, and transformation.

This repository is compatible with the open Agent Skills standard. Tidal does not claim ownership of the “Agent Skills” brand.

## Available skills

| Skill | Version | Description |
|-------|---------|-------------|
| [`tidal-tools`](./skills/tidal-tools/) | 0.3.2 | Install/auth Tidal Tools, custom fields, create Accelerator apps, `tidal analyze code`, and portfolio context for AWS Transform handoffs |

## Scope (`tidal-tools` v0.3.2)

**In scope:** install → auth → optional custom fields → create-app (`POST /api/v1/apps/import` / `tidal request`) → `tidal analyze code` / portfolio business context for **AWS Transform**.

## Source of truth

> Canonical skill text is maintained by Tidal. This repo is a release mirror. Drive-by edits may be closed; open an issue for requested changes.

## Install

### skills CLI (fastest)

```bash
npx skills add tidalmigrations/agent-skills --skill tidal-tools
```

### Cursor

```bash
cp -R skills/tidal-tools ~/.cursor/skills/
# or project-local: .cursor/skills/
```

### Claude Code

```bash
cp -R skills/tidal-tools ~/.claude/skills/
# or project-local: .claude/skills/
```

### Kiro (GitHub subdirectory import)

Import a skill → GitHub:

`https://github.com/tidalmigrations/agent-skills/tree/main/skills/tidal-tools`

Or copy the local `skills/tidal-tools` folder into `~/.kiro/skills/` / `.kiro/skills/`.

## Prerequisites

- Tidal Tools CLI — preferred: `curl https://get.tidal.sh/unix | bash` (Linux/macOS). Offline binaries: https://get.tidal.sh/tidal-win-64-latest, https://get.tidal.sh/tidal-win-32-latest, https://get.tidal.sh/tidal-macos-arm64-latest, https://get.tidal.sh/tidal-macos-64-latest, https://get.tidal.sh/tidal-linux-64-latest, https://get.tidal.sh/tidal-linux-32-latest. Prefer https://get.tidal.sh/AGENTS.md when available.
- Docker (Linux containers) for `tidal analyze code`
- Accelerator workspace credentials (`tidal login`)
- Guides: [https://guides.tidal.cloud](https://guides.tidal.cloud)

## Links

- Installer: [https://get.tidal.sh](https://get.tidal.sh)
- Guides: [https://guides.tidal.cloud](https://guides.tidal.cloud)
- Agent Skills standard: [https://agentskills.io](https://agentskills.io)

## License

Apache-2.0 — see [LICENSE](./LICENSE).
