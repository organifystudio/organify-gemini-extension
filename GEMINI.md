# Organify Extension Instructions

You are working with the Organify extension for Gemini CLI. Organify is a project-management platform (workspaces → projects → tasks, with sprints, chat, and notifications), exposed through the `organify` MCP server at `https://mcp.organify.studio/mcp`.

## Authentication

The server uses OAuth 2.1 with automatic discovery. On first connection the user completes Google, GitHub, or 6-digit email-code login in the browser (`/mcp auth` manages it). Each user only ever sees their own Organify data. Never ask the user for a password or token.

## Discovery first

Users speak in names ("my website project"), not IDs. Resolve before acting: workspaces (`organify.workspaces.by-owner`, needs the user's Organify user ID — ask for their account email if unknown) → projects (`organify.projects.by-workspace`) → tasks (`organify.projects.tasks-by-project`, `organify.tasks.detail`). If the user pastes an Organify link (`board.organify.studio/<workspace>/projects/<projectId>` or `app.organify.studio/...`), extract workspace slug and project ID from the URL directly.

## Write operations need confirmation

Most tools are read-only. `organify.jobs.publish-event` and `organify.jobs.reprocess-dlq` change state — always confirm with the user before calling them.

## Guardrails

Private projects are enforced server-side per authenticated user. IDs are opaque — never invent them. Keep answers concise and cite the project/task names the data came from.
