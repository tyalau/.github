# ⚙️ Central Configurations (.github)

This repository serves as the **Central Command Center** for my GitHub account. It contains global defaults, CI/CD configurations, and community health files used across all my projects.

## 📦 What's Shared

| File | Purpose |
| --- | --- |
| [`.github/labeler.yml`](./.github/labeler.yml) | Label rules for Pull Requests |
| [`.github/workflows/labeler.yml`](./.github/workflows/labeler.yml) | Reusable workflow that applies those labels |

## 🏷️ Global Labeling Logic

The labeler config defines how Pull Requests are categorized:

| Label | Applied when |
| --- | --- |
| `dev ops` | Files under `.github/` change |
| `changeset` | Files under `.changeset/` change |
| `dependencies` | `package.json` or `pnpm-lock.yaml` change |
| `feature` | Branch name starts with `feat/` |
| `refactor` | Branch name starts with `refactor/` |
| `bug` | Branch name starts with `fix/` |
| `docs` | Branch name starts with `docs/`, or any `.md` file changes |

## 🚀 Use in New Projects

Call the reusable workflow from `.github/workflows/labeler.yml` in your project:

```yaml
name: Pull Request Labeler

on:
  pull_request:
    types: [opened, synchronize, reopened]

jobs:
  labeler:
    permissions:
      contents: read
      pull-requests: write
    uses: tyalau/.github/.github/workflows/labeler.yml@main
```

The caller must grant `contents: read` and `pull-requests: write`, since a reusable workflow cannot exceed the permissions of its caller.

### Using a project-specific config

By default the shared [`labeler.yml`](./.github/labeler.yml) is used. To use a config from the calling repository instead, pass `configuration-path`:

```yaml
jobs:
  labeler:
    permissions:
      contents: read
      pull-requests: write
    uses: tyalau/.github/.github/workflows/labeler.yml@main
    with:
      configuration-path: .github/labeler.yml
```

## 📌 Notes

- Keep this repository as the single source of truth for labels and reusable workflows.
- Callers reference `@main`, so updates to the labeler config or workflow apply automatically on the next PR event. Pin to a tag or commit SHA if you need stability.
- Labels are synced (`sync-labels: true`), so labels that no longer match a rule are removed from the PR.
