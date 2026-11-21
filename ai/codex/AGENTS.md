# AGENTS

We are here to create smart and automated workspace. You provide simple and elegant solutions to the problems user face with Fedora workstation.

You do not have sudo permission so you can ask the user to run the command. If destructive highlight it.

We try to minimize the clutter in the /home/varun hence we always work in the /home/varun/Documents/workspace (Developer Workspace)

If working with scripts then you can create here /home/varun/Documents/workspace/github/scripts

The following tools are accessible:

- Btop
- gws
- netlify-cli
- android sdk
- vscode
- java
- fnm

## ALWAYS

- Add section what you need from user like blockers, external setup etc. Keep it short concise and also add guidance how it can be done.

- Add section what next.

# Global preferences (Next.js)

## Component structure
- `components/ui/` — base UI wrappers (Base UI / Radix UI primitives)
- Atomic structure alongside: `components/atoms/`, `components/molecules/`, `components/organisms/`
- Build dumb (presentational, no data-fetching) components as first priority; keep logic in hooks/containers.

## API structure
- Always create an Axios instance (`lib/axios.ts` or similar) — supports multiple backend instances later.
- Maintain `api/endpoints.ts` — export endpoint constants grouped per Axios instance/backend.
- Per-feature folders under `api/<feature>/`:
  - `api.ts` — simple exports of functions using the Axios instance
  - `query.ts` — TanStack Query queries & mutations
  - `types.ts` — request/response types
  - `helpers.ts` — API-related transformation functions

# Docs structure (per project)

Organize project documentation under `docs/`:

```
docs/
├── plans/
│   ├── plan.md          # index table
│   ├── <title-dd-mm-yyyy>.md   # actual plan files
│   └── done/            # completed plans moved here
├── todos/
│   ├── todo.md          # index table
│   ├── <title-dd-mm-yyyy>.md   # actual todo files
│   └── done/            # completed todos moved here
└── knowledge-base/
    └── <title-dd-mm-yyyy>.md   # knowledge files (flat, no index)
```

## Index file (`plan.md` / `todo.md`)
A markdown table with columns:

| File | Description | Status |
|------|-------------|--------|
| `<title-dd-mm-yyyy>.md` | short, concise summary of what it is about | done / preparing / ongoing / blocked / cancelled / none |

- Status is one of: `done`, `preparing`, `ongoing`, `blocked`, `cancelled`, `none`
- When a plan/todo is done, move its file into `done/` and update the index path + status

## File metadata header
Every plan, todo, and knowledge-base file starts with:

```markdown
---
model: <model name used>
last-updated: <dd-mm-yyyy>
user: <user name making the change>
---
```

Update `last-updated` (and `model`/`user` if changed) on every edit, so it is trackable who did what.
