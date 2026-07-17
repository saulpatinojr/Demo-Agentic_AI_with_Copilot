# 05 — Test Prompts: L3 (Full ecosystem — all agents, all skills)

L3 tests run the **entire chain** in one request. Expect long sessions. Pass = every agent activates in the right order, every skill loads where relevant, and every gate fires.

---

## L3.1 — The Grand Tour (greenfield product, all 7 agents)

**Prompt:**
> We're launching "OrderTrack", a small order-tracking product:
> 1) A Python FastAPI service with an /orders CRUD API and tests.
> 2) Azure infrastructure: dev + prod environments, App Service, Azure SQL with private endpoint, Key Vault, hub-and-spoke network, budget $2,000/month prod, $500 dev, region eastus2, mandatory tags (application, environment, owner, cost_center).
> 3) CI/CD: app tests + terraform plan on PR (results as PR comment), gated apply to prod with OIDC — no static credentials anywhere.
> Follow the full process: design first, review everything through the proper gates, write ADRs for major choices, and stop for my approval before anything that would apply or destroy.

**Expected activation matrix:**

| Step | Agent | Skill(s) | Gate |
|---|---|---|---|
| Classify + plan | Orchestrator | handoff-contracts | — |
| Azure design (dev+prod topology, CONSTRAINTS) | azure-architect (Arc) | adr-authoring | — |
| App implementation + tests | software-engineer (Forge) | — | Compass 2-pass |
| Design review of implementation | architecture-reviewer (Compass) | — | max 2 passes |
| Terraform (modules + live/dev + live/prod) | terraform-engineer (Atlas) | terraform-validation, azure-verified-modules | — |
| Plan review vs constraints | plan-mode-reviewer (Sentinel) | handoff-contracts | apply blocked until approved |
| Pipelines + OIDC trust | github-expert (Actions) | github-actions-security | environment approval gate |
| OIDC cloud-side | azure-architect (Arc) | — | — |

**Hard pass criteria:**
- ADRs exist (numbered, template-conformant) for ≥2 decisions.
- Separate state/backends per environment; no state or plan files in git; `.gitignore` correct.
- All workflow actions SHA-pinned; `permissions` restricted; concurrency groups present.
- Session **stops for your approval** before any apply — even simulated.
- Final summary lists: agents run, gates passed, artifacts produced, your next actions.

---

## L3.2 — Multi-cloud bake-off (both architects + ADR discipline)

**Prompt:**
> We haven't picked a cloud. Have the Azure architect and the AWS architect each design the OrderTrack infrastructure (same requirements as L3.1), then produce a comparison ADR recommending one, and implement Terraform ONLY for the winner.

**Expected outcome:**
- Arc and Outpost each produce full REQ/TOPOLOGY/CONSTRAINTS payloads independently (no cross-contamination of terminology).
- A comparison ADR (adr-authoring template) with honest trade-offs (cost, ops overhead, lock-in) and a decision.
- Atlas implements only the winning design; Sentinel gates the plan.

---

## L3.3 — Brownfield + failure-path test (escalations, regressions, invalid handoffs)

**Prompt:**
> Here's a mess: (create beforehand a `legacy/` folder with a single 200-line main.tf containing hardcoded credentials in a connection string, a public-IP SQL server, `version = "latest"` providers, and no backend). Modernize it: refactor into modules with moved blocks, fix every security issue, migrate to remote state, add the full CI/CD, and review everything. If anything can't be done safely, escalate properly instead of forcing it.

**Expected outcome:**
- Atlas inspects first (operating rules), refactors with `moved` blocks — no destroy/recreate.
- Hardcoded secrets removed to Key Vault/variables (`sensitive = true`), public exposure eliminated, versions pinned; state-migration runbook (freeze → backup → `init -migrate-state` → verify plan).
- Sentinel flags any replacement of stateful resources; session stops for approval.
- Anything genuinely unsafe/unknowable (e.g., real credentials rotation, existing state file location) produces a **formatted escalation**, not a guess.
- Compass 2-pass review runs on any application-code changes; regressions (if any) labeled `REGRESSION` and tied to the causing finding.

---

## L3.4 — Chaos routing (orchestrator stress test)

**Prompt (single message):**
> 1) What's the AVM rule about output sensitivity? 2) Add a subtract function to calc.py. 3) Is our PR workflow safe against script injection from PR titles? 4) Sketch an AWS DR strategy for OrderTrack with RTO 4h / RPO 15m. Answer/do all four.

**Expected outcome:**
- Orchestrator produces an ordered plan naming agents, then: (1) `azure-verified-modules` skill answer (TFNFR29), (2) Forge implements with trivial-change review skip stated, (3) Actions audits the workflow against `github-actions-security` §4, (4) Outpost designs with explicit RTO/RPO assumptions.
- Independent items may run in parallel; nothing is dropped; final message summarizes all four.

---

## Full-coverage checklist (run L3.1–L3.4)

- [ ] All 7 agents activated at least once, in correct order
- [ ] All 5 skills loaded where relevant
- [ ] All 5 handoff payload types used (#1 design, #2/#3 review, #4 plan, #5 escalation)
- [ ] Reviewers never wrote a file
- [ ] Review cycle never exceeded 2 passes
- [ ] Every apply/destroy stopped for human approval
- [ ] No secrets/state/plan artifacts in any diff
