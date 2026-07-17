# 03 — Test Prompts: L1 (Simple, single agent)

Each L1 test exercises exactly **one agent** or **one skill**. Run them in a fresh session. "Pass" means the right agent activates and the output matches the expected shape.

---

## L1.1 — Azure Architect (Arc)

**Prompt:**
> Design a small Azure landing zone for a single dev web app. Budget: $500/month. Region: eastus2. No on-prem connectivity.

**Expected outcome:**
- Routed to `azure-architect` (not answered inline, no Terraform written).
- Output includes structured **REQ**, **TOPOLOGY** (JSON/YAML: subscription/RG, VNet, subnets), and **CONSTRAINTS** (region list, budget cap, mandatory tags).
- States assumptions (SLA/RTO/RPO) explicitly; no public RDP/SSH anywhere.
- Ends with a handoff offer to `terraform-engineer`.

**Fail signals:** Terraform code in the answer; AWS terminology; no CONSTRAINTS block.

---

## L1.2 — AWS Architect (Outpost)

**Prompt:**
> Design an AWS architecture for a serverless API (Lambda + API Gateway + DynamoDB), prod-grade, us-east-1, budget $1,000/month.

**Expected outcome:**
- Routed to `aws-architect`.
- Well-Architected pillars referenced; multi-account guidance (or explicit single-account risk note); REQ/TOPOLOGY/CONSTRAINTS payloads; ADR for at least one choice (e.g., DynamoDB vs Aurora Serverless).

---

## L1.3 — Terraform Engineer (Atlas)

**Prompt:**
> Write a provider-agnostic Terraform module skeleton called `network` with typed variables for environment, name, tags, and cidr_block, plus outputs and a README.

**Expected outcome:**
- Routed to `terraform-engineer`; files created (`main.tf`, `variables.tf`, `outputs.tf`, `versions.tf`, `README.md`).
- Variables typed, described, with validation on `environment`.
- Runs (or recommends, if tools missing) `terraform fmt` / `init -backend=false` / `validate` — the `terraform-validation` skill sequence.

---

## L1.4 — GitHub Expert (Actions)

**Prompt:**
> Create a CI workflow that runs terraform fmt/validate and tflint on pull requests to main.

**Expected outcome:**
- Routed to `github-expert`; `.github/workflows/*.yml` created.
- Global `permissions: contents: read`; actions pinned to full 40-char SHAs with version comments; readable job/step names; concurrency group present.
- The `github-actions-security` checklist visibly applied.

**Fail signals:** `uses: actions/checkout@v4` (tag, not SHA); missing permissions block.

---

## L1.5 — Software Engineer (Forge) + review skip logic

**Prompt:**
> Create a Python file `calc.py` with an `add(a, b)` function and a test.

**Expected outcome:**
- Routed to `software-engineer`; implements + tests.
- **Correctly skips** the architecture review (trivial-change skip condition) and states the skip reason.

---

## L1.6 — Architecture Reviewer (Compass) — direct + read-only proof

**Prompt (after L1.5):**
> Ask the architecture-reviewer to review calc.py and also to fix anything it finds.

**Expected outcome:**
- Review runs; findings (if any) are suggestions with IDs (F1…).
- The reviewer **declines to edit**: "I'm a read-only reviewer…" and directs fixes to the software-engineer.

---

## L1.7 — Plan Mode Reviewer (Sentinel) — direct

**Prompt:**
> Review this plan before I implement: "Add a /users REST endpoint reading directly from the DB in the route handler, no tests, ship today."

**Expected outcome:**
- Numbered issues (1, 2, …) each with lettered options (A, B, …), concrete trade-offs, an opinionated recommendation listed first, and a request for your decision.

---

## L1.8 — Skill trigger check (AVM)

**Prompt:**
> I'm writing an Azure Terraform module that needs AVM certification. What provider version constraints must I use?

**Expected outcome:**
- `azure-verified-modules` skill loads; answer cites azurerm `>= 4.0, < 5.0`, azapi `>= 2.0, < 3.0`, `~>` style constraints — from the skill, not generic memory.

---

## Scorecard

| Test | Agent activates | Output shape correct | Gates respected |
|---|---|---|---|
| L1.1–L1.8 | ☐ | ☐ | ☐ |

All 8 pass → proceed to [L2](04-Test-Prompts-L2.md).
