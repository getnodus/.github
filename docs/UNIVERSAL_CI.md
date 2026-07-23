# Universal CI

Universal CI is the opt-in stack-independent baseline for `Intuitumxyz/solo`,
`Intuitumxyz/leo`, and `Intuitumxyz/Diffuse`. Storing it centrally does not
cause it to run in other repositories.

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

Selected repository behavior:

| Repository | Local checks |
|---|---|
| `solo` | Add Universal CI on `self-hosted`; keep existing core, extension, Copilot, component, and package checks. |
| `leo` | Add Universal CI on `self-hosted`; keep existing Node validation and deployment workflows. |
| `Diffuse` | Add Universal CI on `self-hosted` after the currently empty repository receives its first commit. |

No Universal CI caller should be added to other repositories.

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
    with:
      runner: self-hosted
```

## Rollout

The public `.github` repository makes the reusable workflow accessible to the
selected private repositories, but it does not inject or run the workflow
automatically. Add the thin caller only to `solo`, `leo`, and `Diffuse`.

Repositories that did not previously run Gitleaks should receive one manual
full-history run after adoption. Normal PR and push runs stay incremental so
large repositories do not repeatedly rescan their entire history.
