---
name: github-actions-security
description: GitHub Actions security hardening checklist. Use whenever writing or reviewing workflow YAML — covers SHA pinning, least-privilege permissions, OIDC federation, secret handling, injection defense, and concurrency safety.
---

# GitHub Actions Security Hardening

Apply every item when authoring or reviewing any `.github/workflows/*.yml` file.

## 1. Pin actions to immutable SHAs

Every third-party action MUST be pinned to a full 40-character commit SHA with a version comment:

```yaml
uses: actions/checkout@11bd71901bbe5b1630ceea73d27597364c9af683 # v4.2.2
```

Never `@v4`, never `@main`. Configure Dependabot (`package-ecosystem: github-actions`) to keep SHAs updated.

## 2. Least-privilege token permissions

Every workflow starts with a restrictive global block, elevated only per-job:

```yaml
permissions:
  contents: read
jobs:
  deploy:
    permissions:
      id-token: write   # OIDC only where needed
      contents: read
```

## 3. OIDC — never static cloud credentials

- AWS: `aws-actions/configure-aws-credentials` with `role-to-assume`
- Azure: `azure/login` with `client-id`/`tenant-id`/`subscription-id` (federated credential)
- GCP: `google-github-actions/auth` with `workload_identity_provider`

Reject any workflow that reads `AWS_SECRET_ACCESS_KEY`, `AZURE_CLIENT_SECRET`, or a GCP JSON key from secrets. Always output the required cloud-side trust policy alongside the workflow.

## 4. Injection defense

- Never interpolate untrusted input (`github.event.*` titles, bodies, branch names) directly into `run:` — pass through `env:` first.
- Avoid `pull_request_target` unless the job never checks out or executes fork code; if unavoidable, scope it and document why.
- Mask dynamic secrets: `echo "::add-mask::$VALUE"`.

## 5. Blast-radius controls

- `concurrency` group on every deploy/apply workflow (`cancel-in-progress: false` for Terraform to avoid state-lock orphans).
- `environment:` with required reviewers for anything touching prod.
- `needs:` chains so apply never runs before plan/scan/test pass.
- Cleanup step with `if: cancelled() || failure()` to release state locks / send alerts.

## 6. Governance

- `CODEOWNERS` covering `.github/workflows/**` (workflow changes require platform-team review).
- Branch protection: required status checks, no force push, required PR review.
- Enable secret scanning + push protection and Dependabot alerts on the repo.

## 7. Lint before shipping

```bash
actionlint .github/workflows/*.yml
zizmor .github/workflows/*.yml   # if available — static security analysis for Actions
```

A workflow is done only when actionlint is clean and every checklist item above is satisfied or explicitly waived with a reason.
