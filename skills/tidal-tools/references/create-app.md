# Create application — blessed path

## Public contract

Guide: https://guides.tidal.cloud/import-apps.html

```
POST https://[subdomain].tidal.cloud/api/v1/apps/import
Authorization: bearer <token>
Content-Type: application/json

{"apps":[ { "name": "...", ... } ]}
```

Response: JSON **array** of application objects; use `id` as `tidal analyze code --app-id`.

Tenant schema of record: `https://[subdomain].tidal.cloud/api_docs`  
API base: `https://[subdomain].tidal.cloud/api/v1/`

## CLI wrapper

Go Tidal Tools (`tidal request`):

- Sends file contents or stdin as the HTTP body (no extra wrapping).
- Auth from `tidal login` / config / env.

```bash
tidal request -X POST /api/v1/apps/import /tmp/tidal-app-import.json
```

Minimal body:

```json
{"apps":[{"name":"My app","description":"optional"}]}
```

With custom fields (keys must match existing custom `Field.name` for `model_type=apps`):

```json
{
  "apps": [
    {
      "name": "My app",
      "description": "optional",
      "custom_fields": {
        "SCR Assessment Note": "Ready for analyze"
      }
    }
  ]
}
```

## Alternate (also in CLI help)

`tidal request -X POST /api/v1/apps app.json` posts whatever is in `app.json`. Prefer **`/api/v1/apps/import`** with the documented `apps` array so agents stay aligned with the public import guide.

## Verification notes (2026-09-22)

- Confirmed in repo: `tidal/cmd/request/request.go` POSTs raw JSON payloads to the given path.
- Confirmed CLI help examples for `tidal request` POST + pipe to sync endpoints.
- Live `POST /api/v1/apps/import` verified with `tidal request` against Accelerator: created app returned in a JSON array with `id` and applied `custom_fields`.
- Live `POST /api/v1/fields` verified in the same session before import (see `custom-fields.md`).
