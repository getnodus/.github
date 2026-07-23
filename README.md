# Intuitum

Shared GitHub defaults and automation for the Intuitum organization.

- `default.json` is the shared Renovate preset: `github>Intuitumxyz/.github`.
- `claude.yml` provides trusted-collaborator Claude automation.
- `shared-checks.yml` is the selected repository-hygiene, Actions-validation,
  and secret-scanning workflow for `solo` and `leo`; empty `diffuse` can opt in
  when it has source to check.
- `ci.yml` runs those shared checks against this repository on GitHub-hosted
  runners.

Shared workflows are opt-in. Build, test, deployment, and release automation
remain with the repositories they serve.
