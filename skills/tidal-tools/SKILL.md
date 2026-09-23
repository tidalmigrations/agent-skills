---
name: tidal-tools
description: >-
  Use when installing or authenticating Tidal Tools, discovering domains/URLs,
  importing or syncing apps/servers/databases into Accelerator, creating custom
  fields, running tidal analyze code, exporting or fetching portfolio business
  context for AWS Transform handoffs, or exploring CLI actions via tidal help.
license: Apache-2.0
compatibility: Requires Tidal Tools CLI, Docker (for analyze code), network to *.tidal.cloud
metadata:
  author: Tidal
  version: "0.3.2"
  public: "true"
---

# Tidal Tools

Walk a user or agent through **Tidal Tools + Accelerator** for discovery, inventory import/sync, source-code analysis, and portfolio context in modernization / AMA-style flows. Target users are **AWS SI partners** who use Tidal Accelerator for discovery and assessment before modernization, migration, and transformation. Accelerator inventory and Source Code Readiness results are useful inputs to **AWS Transform**.

**In scope:** install, auth, discovery, import/sync, optional custom fields, create application records, `tidal analyze code`, export/request portfolio context, show where results land. For the full command surface, run `tidal help` (then `tidal <command> --help`) — do not invent subcommands or schemas.

Prefer documented CLI/API only. Do not invent field or import schemas — discover them from `tidal help`, public guides, and the tenant API docs when needed.

Install and guides:

- **Installer:** https://get.tidal.sh
- https://guides.tidal.cloud/tidal-tools.html
- https://guides.tidal.cloud/discover.html
- https://guides.tidal.cloud/sync-servers.html
- https://guides.tidal.cloud/analyze-source-code.html
- https://guides.tidal.cloud/code-analysis-overview.html
- https://guides.tidal.cloud/modernization-assessment.html
- https://guides.tidal.cloud/import-apps.html

Workspace API (schema of record):

- Base: `https://[subdomain].tidal.cloud/api/v1/`
- Interactive docs (authenticated): `https://[subdomain].tidal.cloud/api_docs`

---

## 1. Install and health-check

Do **not** scrape https://get.tidal.sh HTML for install commands. Use the matrix below (or https://get.tidal.sh/AGENTS.md when that file returns 200).

1. Install Tidal Tools CLI:

   **Linux / macOS (preferred):**

   ```bash
   curl https://get.tidal.sh/unix | bash
   ```

   **Offline / direct binaries:**

   | Platform | URL |
   | --- | --- |
   | Windows 64-bit | https://get.tidal.sh/tidal-win-64-latest |
   | Windows 32-bit | https://get.tidal.sh/tidal-win-32-latest |
   | macOS arm64 | https://get.tidal.sh/tidal-macos-arm64-latest |
   | macOS x86_64 | https://get.tidal.sh/tidal-macos-64-latest |
   | Linux 64-bit | https://get.tidal.sh/tidal-linux-64-latest |
   | Linux 32-bit | https://get.tidal.sh/tidal-linux-32-latest |

   Details: https://guides.tidal.cloud/tidal-tools.html

2. Install **Docker** (Linux containers). Required for `tidal analyze code`.
3. Verify:

```bash
tidal doctor
tidal version
tidal help
```

If https://get.tidal.sh/AGENTS.md exists, prefer it for machine-readable install guidance; otherwise use this matrix.

Use `tidal help` (and nested `--help`) to see available discovery, sync, import, export, and analyze actions before inventing a path.
Fix anything `tidal doctor` reports before continuing.

---

## 2. Authenticate

Prefer interactive login (password not persisted):

```bash
tidal login
tidal ping
```

Alternatives (see guides): `tidal config set tidal.email|password|url`, env `TIDAL_EMAIL` / `TIDAL_PASSWORD` / `TIDAL_URL`, or `--tidal-*` flags.

`tidal ping` must succeed before create-app or online analyze.

---

## 3. Custom fields (when you need assessment metadata)

If you need to store assessment or business context on Application / Server / Database records (for later AWS Transformation handoff or portfolio views), **create the custom field definitions first**. Values keyed to unknown field names are silently dropped on import.

### Create a field definition

`POST /api/v1/fields` with a `field` wrapper. Confirm enums and required attributes in the tenant `api_docs` before inventing types.

```bash
cat > /tmp/tidal-field-create.json <<'JSON'
{
  "field": {
    "name": "SCR Assessment Note",
    "model_type": "apps",
    "field_type": "text",
    "field_category": "Other",
    "hint": "Short note from source-code readiness assessment"
  }
}
JSON

tidal request -X POST /api/v1/fields /tmp/tidal-field-create.json
```

Common `model_type` values: `apps`, `servers`, `database_instances`.  
Common `field_type` values: `text`, `number`, `checkbox`, `dropdown`, `multiline`, `date`, `currency`, `multiselect`.

List existing fields before creating duplicates:

```bash
tidal request '/api/v1/fields?model_type=apps'
```

See `references/custom-fields.md` for value rules and update paths.

---

## 4. Create an application record (when none exists)

Agents must **not invent** field schemas. Use the documented import endpoint and tenant `api_docs`.

### Blessed path (Go CLI)

`tidal request` POSTs the JSON body as-is (file args or stdin) to the given API path. Use **`POST /api/v1/apps/import`** with an `apps` array — same shape as https://guides.tidal.cloud/import-apps.html.

```bash
cat > /tmp/tidal-app-import.json <<'JSON'
{
  "apps": [
    {
      "name": "Equinox demo",
      "description": "AMA demo sample for source code analysis",
      "custom_fields": {
        "SCR Assessment Note": "Equinox sample — currency TBD after analyze"
      }
    }
  ]
}
JSON

tidal request -X POST /api/v1/apps/import /tmp/tidal-app-import.json
```

Or:

```bash
echo '{"apps":[{"name":"Equinox demo","description":"AMA demo sample"}]}' \
  | tidal request -X POST /api/v1/apps/import
```

**Success:** JSON array of created apps. Capture **`id`** from the first element (integer). That is `--app-id`.

Optional fields documented on the import guide include `description`, `urls`, `custom_fields`, `transition_overview`, `transition_type`, `source_code_location`, and nested `servers` — only send fields you have real values for. `custom_fields` keys must match an existing custom `Field.name` for `model_type=apps`.

### Equivalent curl (if CLI request is unavailable)

```bash
curl -X POST "https://[subdomain].tidal.cloud/api/v1/apps/import" \
  -H "authorization: bearer [access_token]" \
  -H "content-type: application/json" \
  -d '{"apps":[{"name":"Equinox demo","description":"AMA demo sample"}]}'
```

Obtain subdomain and token via https://guides.tidal.cloud/ (authentication / get-subdomain flows). Prefer `tidal login` + `tidal request` so agents do not handle raw tokens in chat.

### If an app already exists

Do **not** create a duplicate. Read the app id from the Accelerator URL (`…/apps/<id>/…`) or from the Source Code Readiness panel command, or:

```bash
tidal request /api/v1/apps
```

Pick the matching id from the response — never invent an id. To set custom field values on an existing app, `PUT /api/v1/apps/<id>` with `custom_fields` (see `references/custom-fields.md`).

---

## 5. Obtain sample source (optional demos)

AMA demo-data zip: https://s3.ca-central-1.amazonaws.com/tidal.assets.public/demo-data/demo-data.zip  
Guide: https://guides.tidal.cloud/modernization-assessment.html

| Sample | Use when |
| --- | --- |
| **equinox-project** | Default / fast (Windows-timely) |
| **nop-commerce** | Richer eCommerce sample; on Windows prefer **WSL** for Docker performance |

Optional GitHub shortcut for Equinox only:

```bash
git clone --depth 1 https://github.com/EduardoPires/EquinoxProject.git equinox-project
cd equinox-project
```

Prefer the zip copy when aligning to the public AMA guide narrative.

---

## 6. Run source code analysis

### Online (machine can reach Accelerator)

From the source root (or pass paths):

```bash
cd /path/to/equinox-project
tidal analyze code --app-id <APP_ID>
```

Multiple paths for one app:

```bash
tidal analyze code ./src ./lib --app-id <APP_ID>
```

### Offline → upload

On the air-gapped machine:

```bash
tidal analyze code --offline
# produces code-analysis-*.json (see command output / cwd)
```

On a networked machine:

```bash
tidal analyze code upload ./code-analysis-<...>.json --app-id <APP_ID>
```

Offline staging of Docker deps: `tidal backup` / `tidal restore` — see analyze-source-code guide.

Useful flags (from `tidal analyze code --help`): `--timeout`, `-t/--type`, `-r/--rule`, `-o/--output-dir`, `-f/--output-file`.

---

## 7. Where results land in Accelerator

1. Open the application in Accelerator: `https://[subdomain].tidal.cloud/apps/<APP_ID>/…`
2. Find **Source Code Readiness** (same area that shows the pre-generated `tidal analyze code --app-id …` command).
3. Expect language breakdown by line count and a **currency** score (may show `0` briefly; currency often updates within ~10 minutes after upload).
4. Portfolio rollups: **Insights** under Portfolio (may lag up to 12 hours unless Preferences → Sync Business Analytics → Trigger sync).

Do not invent score interpretations beyond what the UI and https://guides.tidal.cloud/modernization-assessment.html / code-analysis-overview describe.

For AWS Transformation handoffs, report the Accelerator app URL, Source Code Readiness summary, and any custom field values that capture business context — do not fabricate scores or readiness claims.

---

## 8. Agent checklist (happy path)

1. `tidal doctor` → Docker OK  
2. `tidal login` → `tidal ping`  
3. If needed: create custom fields via `POST /api/v1/fields`  
4. Create app via `tidal request -X POST /api/v1/apps/import …` **or** reuse existing id  
5. `cd` into equinox (default) or nop-commerce  
6. `tidal analyze code --app-id N`  
7. Open Source Code Readiness for app `N`  
8. Report: app id, analyze exit status, UI URL, relevant custom fields — no fabricated metrics  

If any step fails, stop and surface the CLI/API error. Do not invent workaround schemas.

---

## References

- `references/create-app.md` — import payload notes and verification status  
- `references/custom-fields.md` — field create + `custom_fields` value rules  
- `references/demo-samples.md` — equinox vs nop-commerce pointers  
