# Universal CI

Universal CI is the stack-independent baseline for every non-empty
`Intuitumxyz` repository.

## What it checks

1. `git diff --check` over the relevant PR or push range catches whitespace
   errors and unresolved conflict markers.
2. Actionlint validates GitHub Actions structure and expressions. ShellCheck,
   Pyflakes, and custom runner-label policy are intentionally repo-local because
   the existing repositories have different shell conventions and runner labels.
3. Gitleaks scans the relevant PR or push commit range with a pinned,
   checksum-verified binary. A root `.gitleaks.toml` is discovered
   automatically. A manual `workflow_dispatch` scans full history.

The workflow checks x86-64 and ARM64 release checksums before running downloaded
binaries. It has only `contents: read` permission.

## What remains repository-specific

Universal CI does not install dependencies or guess commands. Each repository
continues to own:

- lint, typecheck, build, and tests;
- language and package-manager setup;
- deployment and releases;
- merge-queue behavior;
- notifications and agent integrations;
- any non-default runner requirements.

Current local CI:

| Repository | Local checks |
|---|---|
| `solo` | Core, extension, Copilot, component, and package checks on self-hosted runners. |
| `argus` | SwiftPM build/test should be added separately. |
| `intuitum-site` | Node lint and typecheck. |
| `leo-site` | Node build/typecheck/test should be added separately. |
| `leo` | Node typecheck/test plus deployment workflows. |
| `argus.core` | Python 3.12/3.13 Ruff, Pyright, and Pytest matrix. |

## Caller

```yaml
name: Universal CI

on:
  pull_request:
  push:
    branches: [main]
    tags: ['**']
  workflow_dispatch:

permissions:
  contents: read

jobs:
  universal:
    uses: Intuitumxyz/.github/.github/workflows/universal-ci.yml@main
```

`solo` passes `runner: self-hosted` to preserve its current scan capacity.

## Rollout

The public `.github` repository makes the reusable workflow accessible to
private organization repositories, but it does not inject the workflow
automatically. Add the thin caller to each existing repository. The workflow
template makes the same caller discoverable for future repositories.

Repositories that did not previously run Gitleaks should receive one manual
full-history run after adoption. Normal PR and push runs stay incremental so
large repositories do not repeatedly rescan their entire history.
