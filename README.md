# Practice Repo

This repo is my playground / notes for the technologies I learn. It helps me refer back to
sample code snippets and concept definitions when I need them.

## Contents

| Folder           | What's inside                                                       |
| ---------------- | ------------------------------------------------------------------- |
| `ai/`            | AI notes and reusable skills, including GitHub CLI workflows        |
| `assets/`        | Reference images (e.g. HTTP status codes)                            |
| `config/`        | VS Code profile, extensions list, and editor settings                |
| `c/`             | C basics: data types, operators, control flow, arrays, structs       |
| `db/`            | Database notes (placeholder for now)                                 |
| `docker/`        | Docker notes + a compose demo project (React blog + API)             |
| `ds/`            | Data structures practice (linked list)                               |
| `go/`            | Go modules and hello world exercises                                 |
| `js/`            | JavaScript basics, web examples, coding practice, Node.js notes      |
| `linux/`         | Linux notes (WiFi power management, tmux)                            |
| `mongodb/`       | MongoDB notes (01–08) and playground scripts                         |
| `notes/`         | Cross-cutting notes: SOLID, security, Kafka, Next.js, OWASP, etc.    |
| `notes/open-source/` | Open source alternatives to popular SaaS tools                   |
| `python/`        | Python OOP, concepts, libs, and scripts                              |
| `rust/`          | Rust exercises: ownership, borrowing, structs, slices, basics        |
| `sql/`           | SQL notes, MySQL exercise scripts, interview questions               |
| `system-design/` | System design notes, microservices, and question bank                |
| `ts/`            | TypeScript samples and notes                                         |
| `vps/`           | VPS setup notes                                                      |
| `web-dev/`       | HTML/CSS/JS fundamentals                                             |

## Conventions

### Naming

- Folders: lowercase kebab-case (`js-basics/`, `system-design/`, `linked-list/`).
- Files: lowercase kebab-case (`mysql-setup.md`, `js-is-weird.md`). No spaces, no ALL-CAPS.
- Sequential notes use zero-padded numeric prefixes (`01-`, `02-`, ...), e.g. `notes/05-kafka.md`.
- Assets live in an `assets/` subfolder next to the notes that reference them.
- Sub-projects named after their own topic only when the parent already gives context
  (`rust/basics/`, not `rust/rust_basics/`).
- Never rename files inside cloned third-party repos — it breaks their internals.

### Structure

- One top-level folder per topic; notes, code, and assets for a topic live together.
- Cross-cutting topics (security, practices) go in `notes/`.
- Editor setup and machine config go in `config/`, shared images in `assets/`.

### Hygiene

- Build artifacts are not committed: `dist/`, `build/`, compiled binaries, `coverage/`.
- One lockfile per project (pnpm preferred), don't commit `node_modules/`.
- Markdown must pass `markdownlint` (see `.markdownlint.jsonc`): no inline HTML
  (`<br />`), no bare URLs, blank lines around headings/lists/fences.
- Don't commit secrets — `.env*` is gitignored.
