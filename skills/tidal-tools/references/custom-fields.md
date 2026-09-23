# Custom fields — create then set values

Use custom fields when assessment or business context must live on Accelerator Application, Server, or Database records (for example notes that later feed AWS Transformation handoffs).

## Schema of record

- API base: `https://[subdomain].tidal.cloud/api/v1/`
- Authenticated docs: `https://[subdomain].tidal.cloud/api_docs`

Do **not** invent `field_type`, `model_type`, or `field_category` values. Confirm enums in `api_docs` for the tenant.

## Create a field definition

```
POST /api/v1/fields
```

Body requires a `field` wrapper:

```json
{
  "field": {
    "name": "SCR Assessment Note",
    "model_type": "apps",
    "field_type": "text",
    "field_category": "Other",
    "hint": "optional",
    "label": "optional"
  }
}
```

CLI:

```bash
tidal request -X POST /api/v1/fields /tmp/tidal-field-create.json
```

**Success:** `201` + Field JSON (`id`, `name`, `model_type`, `field_type`, …). Duplicate names for the same model typically return `422`.

### Useful list

```bash
tidal request '/api/v1/fields?model_type=apps'
```

Also valid `model_type` filters: `servers`, `database_instances` (and others documented in `api_docs`).

### Common enums (confirm in api_docs)

| Attribute | Typical values |
| --- | --- |
| `model_type` | `apps`, `servers`, `database_instances` |
| `field_type` | `text`, `number`, `checkbox`, `dropdown`, `multiline`, `date`, `currency`, `multiselect` |
| `field_category` | `Other`, plus categories shown in the UI / docs |

For `dropdown` / `multiselect`, include nested `field_options` as documented in `api_docs`.

## Set values on records

Keys in `custom_fields` must match an existing custom field **`name`** for that model. Unknown keys are **silently dropped** (import/update can still succeed with those keys missing from the stored hash).

### On create (import)

```
POST /api/v1/apps/import
```

```json
{
  "apps": [
    {
      "name": "Equinox demo",
      "custom_fields": {
        "SCR Assessment Note": "Verified via tidal request"
      }
    }
  ]
}
```

Same pattern for `POST /api/v1/servers/import` and `POST /api/v1/database_instances/import` with their identity keys.

### On update

```
PUT /api/v1/apps/<id>
```

with a `custom_fields` object. Merge semantics: new keys merge into the existing hash; a `null` value unsets a key (per Accelerator behavior).

### Sync path

`tidal sync apps|servers|dbs` may auto-create missing custom fields before upserting via `/sync` endpoints. For agent-controlled demos, prefer explicit `POST /api/v1/fields` + `/import` so the skill stays aligned with documented HTTP shapes.

## Verification notes (2026-09-22)

- Live `POST /api/v1/fields` with `model_type=apps`, `field_type=text`, `field_category=Other` succeeded via `tidal request`.
- Live `POST /api/v1/apps/import` with matching `custom_fields` returned the app array including the stored custom field value.
