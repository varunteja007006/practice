---
name: github
description: >
  Use GitHub CLI (`gh`) to inspect and manage repositories, forks, sync status,
  topics, descriptions, names, files, and integrations. Use when asked to work
  with GitHub from the terminal. Inspect first; never force-sync or unarchive
  without explicit authorization.
license: MIT
---

# GitHub CLI workflow

Use `gh` for GitHub operations. Replace example owners, repositories, and
branches with the user's actual values. Do not print or expose authentication
tokens.

## 1. Check access and inspect first

```sh
gh auth status
gh repo view OWNER/REPO --json nameWithOwner,description,isPrivate,isFork,defaultBranchRef
```

List a user's repositories, including forks and privacy:

```sh
gh repo list OWNER --limit 1000 \
  --json nameWithOwner,description,isPrivate,isFork
```

Inspect a fork's parent and default branch:

```sh
gh api repos/OWNER/REPO \
  --jq '{name: .full_name, parent: .parent.full_name, fork, default_branch}'
```

Check the actual branch names before comparing or syncing. Never assume both
repositories use `main`.

## 2. Compare a fork with its parent

```sh
gh api 'repos/UPSTREAM/REPO/compare/UPSTREAM_BRANCH...FORK_OWNER:FORK_BRANCH' \
  --jq '{status, ahead_by, behind_by, merge_base: .merge_base.sha}'
```

- `behind` with zero ahead commits is usually a normal fast-forward sync.
- `ahead` does not prove the fork owner authored those commits.
- “No common ancestor” means the histories diverged or were rewritten. Do not
  treat the ahead/behind counts as proof that a force-sync is safe. Inspect
  commit authors and compare important file contents, especially `LICENSE`.
- Check that the comparison targets the intended parent and branches.

## 3. Sync safely

Try a normal sync first; without `--force`, GitHub will not overwrite a
 diverged branch:

```sh
gh repo sync FORK_OWNER/REPO --branch BRANCH
```

Force-sync replaces the fork branch with the parent's branch and discards the
fork's unique history. Use it only after the user explicitly authorizes it and
a review confirms there is nothing to preserve:

```sh
gh repo sync FORK_OWNER/REPO --branch BRANCH --force
```

Verify the fork and parent branch SHAs match afterward:

```sh
gh api repos/FORK_OWNER/REPO/branches/BRANCH --jq '.commit.sha'
gh api repos/UPSTREAM/REPO/branches/BRANCH --jq '.commit.sha'
```

## 4. Edit repository metadata

Inspect existing topics before changing them:

```sh
gh api repos/OWNER/REPO/topics --jq '.names'
```

The topics endpoint replaces the entire topic list. Preserve existing relevant
topics when adding new ones:

```sh
gh api --method PUT repos/OWNER/REPO/topics --input - <<'JSON'
{"names":["python","django","web-app"]}
JSON
```

Rename a repository and set its description in one request:

```sh
gh api --method PATCH repos/OWNER/OLD-NAME --input - <<'JSON'
{"name":"new-name","description":"Short, accurate project summary."}
JSON
```

Renames create GitHub redirects, but still search for old links and check
workflows, deployment providers, and local clone remotes. A GitHub rename does
not guarantee every external integration updates. Only unarchive a repository
when needed and explicitly authorized:

```sh
gh api --method PATCH repos/OWNER/REPO --input - <<'JSON'
{"archived":false}
JSON
```

## 5. Search for stale references and inspect integrations

```sh
gh search code '"OLD-REPO-NAME" user:OWNER' --limit 100 \
  --json repository,path
gh api repos/OWNER/REPO/actions/workflows --jq '.workflows[].path'
gh api repos/OWNER/REPO/hooks --jq '[.[] | {name, active, events}]'
```

Code search can lag behind renames or commits. Verify important files directly.
No custom webhooks or Actions workflows does **not** prove a deployment app is
connected; check its provider dashboard if available.

## 6. Edit a file through the Contents API

Fetch the current SHA, then include it in the update. This prevents silently
overwriting a concurrent edit. `content` must be base64-encoded UTF-8.

```python
import base64
import json
import subprocess

repo = "OWNER/REPO"
path = "README.md"
item = json.loads(subprocess.check_output(
    ["gh", "api", f"repos/{repo}/contents/{path}"]
))
text = base64.b64decode(item["content"]).decode("utf-8")
text = text.replace("old text", "new text")
payload = {
    "message": "Update README",
    "sha": item["sha"],
    "content": base64.b64encode(text.encode()).decode(),
}
subprocess.run(
    ["gh", "api", "--method", "PUT",
     f"repos/{repo}/contents/{path}", "--input", "-"],
    input=json.dumps(payload), text=True, check=True,
)
```

Read the file back after the commit and verify the change.

## 7. Profile pins

GitHub's public GraphQL API can read `pinnedItems`, but it does not expose a
mutation to manage pinned repositories. Do not claim pins were changed via
`gh`; tell the user to use **Customize your pins** on their GitHub profile.

## Operating rules

1. Inspect repo ownership, fork parent, branch, privacy, and archived status.
2. Explain destructive effects before overwrite, deletion, or visibility changes.
3. Make only the requested changes; preserve existing topics and user content.
4. Verify remote state after every mutation and report anything that needs a
   provider dashboard or manual UI action.
