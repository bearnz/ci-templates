# ci-templates

Shared GitHub Actions workflows for bearnz's repos. This repo is private.
Other private repos owned by bearnz can call these workflows (Settings →
Actions → General → Access is set to "Accessible from repositories owned by
the user"). Public repos can't.

## secret-scan

Runs gitleaks over the caller's full git history on every push and PR. Each
repo carries only a small caller, `.github/workflows/secret-scan.yml`:

```yaml
name: secret-scan
on: [push, pull_request, workflow_dispatch]
permissions:
  contents: read
jobs:
  gitleaks:
    uses: bearnz/ci-templates/.github/workflows/secret-scan.yml@v1
```

The canonical copy of that caller lives in `~/.claude/templates/secret-scan/`
on Josh's PC, and the `/secret-scan` Claude skill installs it. Findings are
accepted per repo, by adding the finding's fingerprint to that repo's
`.gitleaksignore`.

## Versioning

Callers follow the `v1` tag. A change lands on `main` through a PR, where
`self-scan` runs it against this repo. After that, Josh moves the tag from a
terminal:

```bash
git fetch origin
git tag -f v1 origin/main
git push --force origin refs/tags/v1
```

That step is deliberately manual. Claude's push guard refuses force pushes, so
callers only pick up a change once Josh decides it's ready. A breaking change
gets a new `v2` tag instead, and callers move over one PR at a time.

## Copyright

Copyright (c) 2026 Josh (github.com/bearnz). All rights reserved.

No licence is granted. You are welcome to read this code and to fork it on
GitHub; you may not use, copy, modify or redistribute it without permission.
See [NOTICE](NOTICE) for the terms, and open an issue if you would like to use
any of it.
