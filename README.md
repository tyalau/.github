# ⚙️ Central Configurations (.github)

This repository serves as the **Central Command Center** for my GitHub account. It contains global defaults, CI/CD configurations, and community health files used across all my projects.

## 🏷️ Global Labeling Logic

The file [`.github/labeler.yml`](./.github/labeler.yml) defines how Pull Requests are categorized.

## 🚀 Use in New Projects

Add a workflow file at `.github/workflows/labeler.yml`:

```yaml
name: 'Pull Request Labeler'
on:
  pull_request:
    types: [opened, synchronize, reopened]

jobs:
  labeler:
    permissions:
      contents: read
      pull-requests: write
    runs-on: ubuntu-latest
    steps:
      - name: Checkout central config repository
        uses: actions/checkout@v5
        with:
          repository: tyalau/.github
          path: .central-config

      - name: Apply labels
        uses: actions/labeler@v6
        with:
          configuration-path: '.central-config/.github/labeler.yml'
          sync-labels: true

```

## 📌 Notes

- Keep this repository as the single source of truth for labels and reusable workflows.
- When `.github/labeler.yml` gets updated, projects inherit the new label behavior automatically on next PR events.
