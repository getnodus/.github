# Intuitumxyz GitHub defaults

This special repository is the organization home for:

- the public organization profile and default community-health files;
- reusable GitHub Actions workflows;
- workflow templates for new repositories;
- the shared Renovate preset;
- organization-wide repository automation guidance.

## Shared automation

| Path | Purpose |
|---|---|
| `.github/workflows/universal-ci.yml` | Opt-in reusable changed-file hygiene, Actions syntax validation, and secret scanning for `solo`, `leo`, and `Diffuse`. |
| `.github/workflows/claude.yml` | Reusable trusted-collaborator `@claude` workflow. |
| `default.json` | Shared Renovate preset, referenced as `github>Intuitumxyz/.github`. |
| `renovate-config.json` | Makes the shared preset discoverable during Renovate onboarding. |

Universal CI deliberately does not install dependencies or guess build, lint,
typecheck, or test commands. Those checks stay in each repository and match its
actual stack. It is callable only and does not run in this repository. See
[`docs/UNIVERSAL_CI.md`](docs/UNIVERSAL_CI.md).

The former `Intuitumxyz/workflow` repository remains a compatibility host until
all callers are migrated and verified. It should be archived, not immediately
deleted, after the compatibility window.
