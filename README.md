# Organify extension for Gemini CLI

Official Organify extension: manage workspaces, projects, tasks, sprints, notifications, and background jobs from Gemini CLI via Organify's hosted MCP server.

- **MCP server:** `https://mcp.organify.studio/mcp` (Streamable HTTP, declared in `gemini-extension.json`)
- **Auth:** OAuth 2.1 with automatic discovery, completed in the browser on first connection (`/mcp auth` — Google, GitHub, or a 6-digit email code). Nothing to install, no API key to paste.
- **Context:** `GEMINI.md` (loaded every session)
- **Skill:** `skills/organify/SKILL.md` (discovery, reporting, and write-op confirmation workflows)
- **Slash commands:** `/organify-status`, `/organify-triage`, `/organify-inbox` (see `commands/`)

## Install

```bash
gemini extensions install https://github.com/organifystudio/organify-gemini-extension
```

Then restart your session and complete the OAuth flow when prompted (`/mcp auth`).

## Tools (16)

Read-only: `organify.system.status`, `organify.users.batch-get`, `organify.workspaces.get-by-id`, `organify.workspaces.summary`, `organify.workspaces.by-owner`, `organify.workspaces.members`, `organify.projects.get-by-id`, `organify.projects.summary`, `organify.projects.by-workspace`, `organify.projects.tasks-by-project`, `organify.tasks.detail`, `organify.tasks.activity`, `organify.notifications.online-users`, `organify.notifications.unread-count`.

Write (the skill instructs agents to confirm with the user first): `organify.jobs.publish-event`, `organify.jobs.reprocess-dlq`.

## Network endpoints & credentials

- The extension calls exactly one network endpoint: `https://mcp.organify.studio/mcp` (+ OAuth discovery/authorize/token on the same host).
- Authentication is per-user OAuth in the browser. The extension ships no secrets, runs no local code or hooks, and collects no telemetry.
- Server-side, private projects are enforced per authenticated user: tools only return data that user may access.

## License

MIT — see [LICENSE](LICENSE).
