# `manifest.json` fields

| Field | Type | Default | What it does |
|---|---|---|---|
| `version` | string | **required** | The current release. Compared numerically, part by part (1.0.10 > 1.0.9). |
| `updater_download_url` | `http(s)` | none | The updater, for people who already have the app. |
| `app_download_url` | `http(s)` | the updater link | The full app, for a machine with nothing installed. |
| `notes` | string | none | One line shown in the prompt. |
| `mandatory` | boolean | **`true`** | Locks the app until they update. Set `false` for a soft release. |
| `optional_for` | string array | `[]` | Versions exempt from the lock, e.g. `["1.2","1.2.1"]`. They get a dismissible prompt. |
| `min_version` | string | none | Forces anything below it even on a soft release. |
| `auto` | boolean | `false` | Lets the app download it itself. Only for links that need no sign-in, so not SharePoint. |
| `sha256` | string | none | Verifies an `auto` download. |
