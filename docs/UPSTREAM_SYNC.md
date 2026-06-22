# Upstream Sync Runbook

This repo is a fork of [VectifyAI/OpenKB](https://github.com/VectifyAI/OpenKB).
The `upstream` remote is already configured. This document describes how to pull
upstream changes without breaking our additions.

## File Ownership

| Layer | Files | Conflicts? |
|---|---|---|
| **Upstream-owned** | `cli.py`, `config.py`, `converter.py`, `indexer.py`, `locks.py`, `schema.py`, `state.py`, `url_ingest.py`, `watcher.py`, `frontmatter.py`, `images.py`, `lint.py`, `log.py`, `tree_renderer.py`, `agent/`, `deck/`, `prompts/`, `skill/` | Possible |
| **Our additions** | `api/`, `storage/`, `services/` | None expected |
| **Shared** | `db/`, `__init__.py`, `__main__.py` | Rare |

Our additions live in **separate subdirectories** — upstream never modifies `api/`,
`storage/`, or `services/`. Conflicts are isolated to upstream-owned files.

---

## Step-by-Step Sync

### 1. Fetch upstream changes

```bash
git fetch upstream
```

### 2. Create a sync branch

Use the naming convention `sync/upstream-YYYY-MM-DD`:

```bash
git checkout develop
git checkout -b sync/upstream-$(date +%Y-%m-%d)
```

### 3. Merge upstream main

```bash
git merge upstream/main
```

### 4. Resolve conflicts (if any)

Conflicts will be in upstream-owned files only. For each conflict:

```bash
git diff --name-only --diff-filter=U   # list conflicted files
```

Resolution strategy:
- **Keep our version** if the upstream change doesn't matter for our use case
- **Keep upstream version** if it's a bug fix or improvement we want
- **Merge both** carefully if both sides have meaningful changes

After resolving:

```bash
git add <resolved-file>
git merge --continue
```

### 5. Run tests

```bash
# Smoke-test the CLI
openkb --help

# Run core package tests (if present)
uv run pytest tests/ -v
```

### 6. Open a PR and tag a new release

```bash
git push origin sync/upstream-$(date +%Y-%m-%d)
# Open PR → develop → review → merge
```

After merge to `main`:

```bash
git checkout main
git pull
git tag vX.Y.Z
git push origin vX.Y.Z
# GitHub Actions will build and publish the pip package automatically
```

### 7. Bump the pin in openkb-platform

In [`ppatel-skillsoft/openkb-platform`](https://github.com/ppatel-skillsoft/openkb-platform):

```bash
# Edit pyproject.toml — change the @vX.Y.Z pin
# Then:
uv sync
docker compose build
docker compose --profile test run --rm isolation-tests
# All 11 isolation tests must pass before merging
```

---

## Upstream Sync Frequency

Recommended: check for upstream releases **quarterly**, or when a specific bug fix
is needed. Avoid syncing too frequently — each merge requires a full test run.

Check upstream releases: https://github.com/VectifyAI/OpenKB/releases

---

## Troubleshooting

**Conflict in `__init__.py`** — usually a version string. Keep ours (hatch-vcs manages it).

**Conflict in `db/`** — upstream may add migrations. Check if their migration conflicts
with ours. If so, renumber ours to maintain sequential ordering.

**`openkb serve` no longer works after merge** — upstream may have changed `cli.py`.
Check if the `serve` command entrypoint is still present and routes correctly to
`openkb.api.app`.
