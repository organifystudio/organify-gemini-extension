---
name: organify
description: "Work with Organify workspaces, projects, tasks, sprints and notifications. Use when the user mentions Organify, their projects, tasks, sprints, workspaces, or pastes an organify.studio link."
---

# Organify

Use the Organify MCP tools to read and manage the user's Organify workspaces, projects, tasks, and sprints.

The server is hosted at `https://mcp.organify.studio/mcp`. If authentication is required, ask the user to complete the OAuth flow (`/mcp auth` — Google, GitHub, or 6-digit email code). Never request that the user paste a password or token into chat.

## Discovery workflow

Resolve user-friendly names to IDs before acting:

1. `organify.workspaces.by-owner` — list the user's workspaces (needs their Organify user ID; ask for their account email if unknown, or start from a pasted link).
2. `organify.projects.by-workspace` — list projects in the workspace; match by name.
3. `organify.projects.summary` — project snapshot with tasks, members, and stats.
4. `organify.projects.tasks-by-project` — tasks of a project.
5. `organify.tasks.detail` — single task with its activity trail and project context.

If the user pastes an Organify link (`board.organify.studio/<workspace>/projects/<projectId>` or `app.organify.studio/...`), extract the workspace slug and project ID directly from the URL instead of listing.

## Reporting workflows

- **Project status**: `organify.projects.summary`, then summarize open vs done tasks, blockers, and members. Synthesize — don't paste raw tool output.
- **Task triage**: `organify.tasks.detail` plus `organify.tasks.activity` for the trail; report state, assignees, and recent events.
- **Workspace overview**: `organify.workspaces.summary` for members, limits, and subscription context.
- **What's new**: `organify.notifications.unread-count` (optionally scoped to a workspace) and `organify.notifications.online-users` for presence.

## Write operations — confirm first

- `organify.jobs.publish-event` — publishes an event into the background-jobs queue.
- `organify.jobs.reprocess-dlq` — moves a job from the dead-letter queue back to active processing.

## Guardrails

- Private projects are enforced server-side: tools only return data the authenticated user may access. If a lookup returns nothing, say so instead of retrying with different IDs.
- IDs are opaque strings — never invent them. Resolve via list/summary tools first.
- Keep responses concise and cite the project/task names the data came from.
