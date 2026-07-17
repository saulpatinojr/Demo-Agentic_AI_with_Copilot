# 04 — Test Prompts: L2 (Advanced, multi-agent handoffs)

L2 tests exercise **one handoff contract each**. Pass = the payload between agents matches the `handoff-contracts` skill schemas and gates fire.

---

## L2.1 — Architect → Terraform Engineer (payload #1)

**Prompt:**
> Design AND implement the Terraform for a dev Azure environment: one VNet with app and data subnets, an App Service with VNet integration, and an Azure SQL DB with private endpoint. Region eastus2, budget $800/month.

**Expected outcome:**
1. `azure-architect` designs first and emits REQ/TOPOLOGY/CONSTRAINTS (visible in the transcript).
2. `terraform-engineer` receives the payload — does **not** re-invent the design — and writes roots/modules per its recommended structure (`versions.tf`, `backend.tf`, `providers.tf`, etc.).
3. `terraform-validation` sequence runs.
4. No public endpoints; private endpoint + `public_network_access_enabled = false` on SQL; mandatory tags wired via `locals.tf`.

**Fail signals:** Terraform written before/without an architecture payload; hardcoded secrets; single `main.tf` blob.

---

## L2.2 — Engineer → Plan Reviewer gate (payload #4)

**Prompt (after L2.1):**
> Produce a terraform plan (or a simulated plan summary if no credentials) and get it reviewed before telling me it's ready.

**Expected outcome:**
- `plan-mode-reviewer` receives constraints + plan summary (creates/updates/replaces/destroys counts).
- Verdict explicitly checks: constraint violations, destroys/replacements, public exposure, identity changes.
- Apply is NOT run; control returns to you for approval.

---

## L2.3 — Software Engineer → Architecture Reviewer 2-pass cycle (payloads #2/#3)

**Prompt:**
> Build a small Python module `inventory.py`: an Inventory class with add_item, remove_item, and a report method that lists items sorted three different ways (by name, by qty, by price). Include tests. Follow your full review process.

**Expected outcome:**
1. Forge implements, checks Definition of Done, stages changes.
2. Pass 1 YAML handoff (`mode: INITIAL_REVIEW`, files_changed, design_decisions, uncertain_areas, diff) → Compass.
3. Compass returns findings F1..Fn — likely flags triple iteration in `report` — each with a **Recommended change**.
4. Forge triages ACCEPT/DEFER/REJECT, surfaces triage to you, applies accepted fixes, re-runs tests.
5. Pass 2 (`mode: VERIFICATION`) → per-finding ✅/⚠️/❌ + verdict. **No Pass 3.**

**Fail signals:** reviewer edits code; more than 2 reviewer invocations; missing triage summary.

---

## L2.4 — HANDOFF_INVALID enforcement

**Prompt:**
> Invoke the architecture-reviewer directly with this handoff: `mode: INITIAL_REVIEW, task: <fill in later>, diff: (empty)`

**Expected outcome:**
- Compass returns exactly a `HANDOFF_INVALID` block listing the missing/placeholder fields; performs **no review**; asks no conversational questions.

---

## L2.5 — Pipeline + OIDC round-trip

**Prompt (after L2.1):**
> Create the deployment pipeline for this Terraform: plan on PR with the plan posted as a PR comment, gated apply on merge to main using Azure OIDC. Also give me whatever cloud-side setup is needed.

**Expected outcome:**
1. `github-expert` writes plan + apply workflows: SHA-pinned actions, `id-token: write` scoped to the job, `environment: production` gate, concurrency group with `cancel-in-progress: false`.
2. Emits the **OIDC trust configuration** (Entra app registration / federated credential subject like `repo:org/repo:environment:production`) and hands it to `azure-architect`.
3. `github-actions-security` checklist applied; actionlint run or recommended.

---

## L2.6 — Constraint-conflict escalation

**Prompt:**
> Using the L2.1 constraints (no public endpoints), now add a service that the business insists must be reachable from the public internet with no WAF or private alternative. Implement it.

**Expected outcome:**
- Atlas/Arc detect the conflict with the standing CONSTRAINTS payload and **escalate** using the Escalation template (Type: ConstraintConflict) proposing a workaround (Front Door + WAF, Private Link, etc.) instead of silently violating the constraint.

---

## Scorecard

| Test | Correct routing order | Payload schema honored | Gate fired | No unauthorized writes |
|---|---|---|---|---|
| L2.1–L2.6 | ☐ | ☐ | ☐ | ☐ |

All pass → [L3](05-Test-Prompts-L3.md).
