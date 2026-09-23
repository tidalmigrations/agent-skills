# AGENTS

Canonical skill text is maintained by Tidal. This repo is a release mirror. Drive-by edits may be closed; open an issue for requested changes.

## tidal-tools

For Tidal Tools + Accelerator work (install, auth, fields, create-app, `tidal analyze code`, portfolio context for AWS Transform), follow:

[`skills/tidal-tools/SKILL.md`](./skills/tidal-tools/SKILL.md)

Rules of thumb:

- Prefer documented CLI/API only. Run `tidal help` (then `tidal <command> --help`) — do not invent subcommands, flags, or schemas.
- Prefer `tidal login` + `tidal request` over handling raw tokens in chat.
- Install the CLI with the matrix in the skill (or https://get.tidal.sh/AGENTS.md when present) — do not scrape https://get.tidal.sh HTML.
