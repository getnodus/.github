# Intuitumxyz GitHub

Organization defaults and narrowly shared automation.

| Path | Purpose |
|---|---|
| `SECURITY.md` | Default vulnerability-reporting policy for organization repositories. |
| `.github/workflows/shared-checks.yml` | Reusable changed-file hygiene, Actions validation, and secret scanning for `solo`, `leo`, and `diffuse` only. |
| `.github/workflows/claude.yml` | Reusable trusted-collaborator `@claude` workflow. |
| `default.json` | Shared Renovate preset, referenced as `github>Intuitumxyz/.github`. |
| `renovate.json` | Renovate configuration for this repository. |

Shared checks are callable only; they do not run automatically in this
repository or across the organization. Thin callers belong only in `solo`,
`leo`, and `diffuse`. They use self-hosted runners and check:

- changed-file whitespace and conflict markers;
- GitHub Actions structure and expressions;
- changed commits for secrets.

Builds, tests, deployments, releases, notifications, and agent automation stay
with the repository they serve. The deprecated `Intuitumxyz/workflow`
repository remains available only until its Renovate and Claude consumers have
migrated here and been verified.
