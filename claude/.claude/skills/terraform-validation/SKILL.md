---
name: terraform-validation
description: Standard Terraform validation and security-scan pipeline. Use whenever Terraform code has been written or modified and needs to be verified before plan/apply — covers fmt, validate, test, tflint, tfsec, checkov, and plan-artifact hygiene.
---

# Terraform Validation Pipeline

Run this sequence after every meaningful `.tf` change. Stop at the first failure, fix, and re-run from the top.

## 1. Local validation (no cloud credentials required)

```bash
terraform fmt -check -recursive     # formatting; run without -check to fix
terraform init -backend=false       # download providers only, no state access
terraform validate                  # schema + reference validation
terraform test                      # if the module has tests/ with *.tftest.hcl
```

## 2. Linting

```bash
tflint --init
tflint --recursive
```

Use the cloud-specific ruleset when the root targets one provider (e.g., `plugin "azurerm"` or `plugin "aws"` in `.tflint.hcl`).

## 3. Security scanning

```bash
tfsec .                              # or: trivy config .
checkov -d . --quiet --compact
```

Findings triage:
- **CRITICAL/HIGH**: must fix or document an explicit, justified suppression inline (`#tfsec:ignore:RULE` / `#checkov:skip=ID:reason`). Never suppress without a reason string.
- **MEDIUM/LOW**: fix when cheap; otherwise list in the PR summary.

## 4. Plan (credentials required)

```bash
terraform init
terraform plan -input=false -out=tfplan
terraform show -no-color tfplan          # human-readable, for review handoff
terraform show -json tfplan > tfplan.json # machine-readable, for policy/cost tools
```

Rules:
- Never commit `tfplan`, `tfplan.json`, `*.tfstate*` — treat plan artifacts as sensitive.
- Hand the plan output to the **plan-mode-reviewer** agent with the CONSTRAINTS payload before any apply.
- If the plan shows destroys or replacements of stateful resources (databases, storage, identity), STOP and surface the risk explicitly.

## 5. Exit criteria

The change is validation-clean when: fmt clean, validate passes, tests pass, tflint clean, no unsuppressed HIGH/CRITICAL scanner findings, and the plan has been reviewed against constraints.
