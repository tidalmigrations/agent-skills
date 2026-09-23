# tidal-tools

Public agent skill for **Tidal Tools + Accelerator**: discovery, inventory import/sync, source-code analysis, and portfolio business context for modernization / AMA-style work and **AWS Transform** handoffs.

**Who it is for:** AWS SI partners and consultants who use Tidal Accelerator for discovery and assessment before modernization, migration, and transformation.

## What it does

1. Install + auth (`tidal doctor`, `tidal login`, `tidal ping`; explore the rest with `tidal help`)
2. Discovery and data import/sync into Accelerator (`tidal discover`, `tidal sync`, documented import APIs)
3. Optionally create custom fields (`POST /api/v1/fields`) for assessment metadata
4. Create an Accelerator application via `POST /api/v1/apps/import` (blessed: `tidal request`)
5. Run `tidal analyze code` (online or offline → upload) → **Source Code Readiness**
6. Export or request portfolio business context for AWS Transform and related modernization workflows

## Layout

```
tidal-tools/
├── SKILL.md                 # required (Agent Skills standard)
├── README.md                # this file
├── reference.md             # retrieval-friendly summary
└── references/
    ├── create-app.md
    ├── custom-fields.md
    └── demo-samples.md
```

This folder is self-contained. Copy it into any agent skills directory — copy this folder into any agent skills directory.

## Install

### skills CLI

```bash
npx skills add tidalmigrations/agent-skills --skill tidal-tools
```


### Cursor

```bash
mkdir -p ~/.cursor/skills
cp -R /path/to/tidal-tools ~/.cursor/skills/
# or project-local:
mkdir -p .cursor/skills
cp -R /path/to/tidal-tools .cursor/skills/
```

Reload the Cursor window, then ask to run a modernization assessment with Tidal Tools (or name `tidal-tools` / `/tidal-tools`).

### Claude Code

```bash
mkdir -p ~/.claude/skills
cp -R /path/to/tidal-tools ~/.claude/skills/
# or project-local:
mkdir -p .claude/skills
cp -R /path/to/tidal-tools .claude/skills/
```

Invoke by naming the skill in the prompt, or via slash command if your Claude Code setup registers skills that way.

### Codex

Codex does not auto-discover Agent Skills `SKILL.md` trees. Copy the folder into the workspace and point Codex at it explicitly, for example in project `AGENTS.md`:

```markdown
For Tidal Tools modernization / source-code readiness work, follow
skills/tidal-tools/SKILL.md
```

Or paste / attach `SKILL.md` for the session.

### Kiro

**Option A — Local folder (fastest)**

1. Copy this folder to your machine (or unzip the pack).
2. In Kiro: **Agent Steering & Skills** → **+** → **Import a skill** → **Local folder**.
3. Select the `tidal-tools` directory (the one that contains `SKILL.md`).
4. Invoke with `/tidal-tools` or ask to run a modernization assessment with Tidal Tools.

**Option B — Copy into Kiro skills dir**

```bash
mkdir -p ~/.kiro/skills
cp -R /path/to/tidal-tools ~/.kiro/skills/
# or for a workspace:
mkdir -p .kiro/skills
cp -R /path/to/tidal-tools .kiro/skills/
```

**Option C — GitHub import**

When this folder is published under a public repo **subdirectory** (not repo root), use Kiro → Import a skill → GitHub and paste the URL to the skill folder or its `SKILL.md`.


## Prerequisites on the machine that runs analyze

- Tidal Tools CLI — `curl https://get.tidal.sh/unix | bash` or offline binaries at https://get.tidal.sh (guide: https://guides.tidal.cloud/tidal-tools.html)
- Docker (Linux containers)
- Accelerator workspace credentials (`tidal login`)
- API schema: `https://[subdomain].tidal.cloud/api_docs` (authenticated)

## Demo samples

- Zip: https://s3.ca-central-1.amazonaws.com/tidal.assets.public/demo-data/demo-data.zip
- Default: `equinox-project` · Fuller: `nop-commerce` (WSL on Windows)

## Version

0.3.2 — purpose covers discovery, import/sync, portfolio context for AWS Transform, and `tidal help` as CLI surface of record; installer https://get.tidal.sh.
