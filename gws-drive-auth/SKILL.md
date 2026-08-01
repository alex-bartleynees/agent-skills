---
name: gws-drive-auth
description: "Auth rules for gws Calendar vs Drive commands. Use whenever running gws calendar or gws drive."
allowed-tools: shell_execute
---

# gws Auth Routing

`gws` has two credential contexts configured on this machine. Always pick the right one — never mix them.

## Calendar (default — no override needed)

Calendar commands use the default environment as-is:

```bash
gws calendar events list --params '{"calendarId": "..."}'
```

This uses the service account already set via `GOOGLE_WORKSPACE_CLI_CREDENTIALS_FILE`. Do not override it for Calendar.

## Drive (always requires explicit override)

Every `gws drive ...` command MUST be prefixed with:

```bash
GOOGLE_WORKSPACE_CLI_CREDENTIALS_FILE="$GWS_DRIVE_USER_CREDS_FILE" gws drive ...
```

Example:

```bash
GOOGLE_WORKSPACE_CLI_CREDENTIALS_FILE="$GWS_DRIVE_USER_CREDS_FILE" \
  gws drive files create --json '{"name": "trip-itinerary.txt", "mimeType": "text/plain", "parents": ["<project-folder-id>"]}'
```

## Folder structure

The top-level Drive workspace for personal-agent usage is:

- **personal-agent** — folder ID `1p63DXUiWCj8C0dwRkmfPT9KR0oWZn2Eh`

This folder may contain multiple project subfolders. For the Auckland move project, use a dedicated subfolder inside `personal-agent` rather than writing files directly into the top-level folder.

## Scope constraint

- Treat `personal-agent` as the allowed top-level Drive workspace.
- Prefer working inside a project-specific subfolder within `personal-agent`.
- Do not attempt to access unrelated Drive locations unless the user explicitly broadens scope or provides another folder ID.

## If a Drive command fails with a 401/403

Do not retry with the Calendar (default) credential — that will not help and Drive access was never granted to the service account. Report the exact error back instead of guessing at a workaround.

## Verification

- Calendar commands should continue to work with the default service-account environment.
- Drive commands must explicitly override `GOOGLE_WORKSPACE_CLI_CREDENTIALS_FILE` to `$GWS_DRIVE_USER_CREDS_FILE`.
- Prefer using known folder IDs directly instead of Drive search/discovery where possible.
- If Drive commands return empty or fail, surface the exact API error instead of switching auth modes implicitly.
