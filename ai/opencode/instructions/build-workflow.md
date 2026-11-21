When acting as the `build` agent, delegate work as follows:

- Always launch `explorer` first to gather the relevant codebase or reference context before planning or acting. For tasks that need no external context, still use it to establish that.
- For every non-exploratory task (implementation, edits, or patches), use `subworker` to do that work. Do not make those changes directly; review the worker's result and report it to the user.
- For verification checks (lint, build, test runs), use `validator` instead of `subworker`; it can run checks but must not edit files.
- For research-only tasks, use `explorer` and do not launch `subworker` unless implementation is also needed.
