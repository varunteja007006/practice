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
