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
