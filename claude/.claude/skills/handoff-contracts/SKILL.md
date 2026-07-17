---
name: handoff-contracts
description: The inter-agent handoff payload schemas for the IaC Agent Ecosystem (REQ, TOPOLOGY, CONSTRAINTS, review handoffs). Use whenever one agent passes work to another — architect to engineer, engineer to reviewer, pipeline to security/FinOps — so handoffs validate 100% of the time.
---

# Inter-Agent Handoff Contracts

Every handoff between agents in this ecosystem uses one of the typed payloads below. **A receiving agent must validate the payload before acting; if required fields are missing, return `HANDOFF_INVALID` with the missing field names — do not guess.**

## 1. Architect → Terraform Engineer (design handoff)

```yaml
REQ: >
  One-paragraph sanitized business/technical requirement.
TOPOLOGY:            # JSON/YAML structural map
  environment: prod
  regions: ["eastus2"]
  network: { vnets: [...], subnets: [...] }
  services: [...]
CONSTRAINTS:
  allowed_regions: ["eastus2", "centralus"]
  require_zone_redundancy: true
  max_monthly_budget: 5000
  mandatory_tags: [application, environment, owner, cost_center]
ADRS:
  - "0001-hybrid-network-topology.md"
```

Required: REQ, TOPOLOGY, CONSTRAINTS. ADRS optional but recommended.

## 2. Software Engineer → Architecture Reviewer (Pass 1)

```yaml
mode: INITIAL_REVIEW
task: <one-paragraph summary of request + plan>
files_changed:
  - path: <file>
    summary: <one line>
design_decisions: <non-obvious choices and why; "none" if trivial>
uncertain_areas: <second-opinion items; "None — confident in all choices" if none>
diff: |
  <git diff --staged output>
```

## 3. Software Engineer → Architecture Reviewer (Pass 2)

```yaml
mode: VERIFICATION
accepted_findings:
  - id: F1
    title: <one line>
    fix_summary: <one line>
diff: |
  <diff of fix-application changes only>
```

## 4. Terraform Engineer → Plan Mode Reviewer (plan gate)

```yaml
mode: PLAN_REVIEW
constraints: <the CONSTRAINTS block from the architect, verbatim>
plan_summary:
  creates: <n>
  updates: <n>
  replaces: <n>
  destroys: <n>
plan_text: |
  <terraform show -no-color tfplan output>
```

Reviewer must verify: no constraint violations, no unexplained destroys/replacements, no new public exposure, no identity-scope expansion.

## 5. Escalation (any agent → upstream agent or human)

```text
### ESCALATION - [TIMESTAMP]
**Type**: Block | Access | Gap | Technical | ConstraintConflict
**Context**: <complete situation with data/logs>
**Solutions Attempted**: <list with results>
**Root Blocker**: <the single impediment>
**Impact**: <effect on task and downstream work>
**Recommended Action**: <specific steps needed>
```

## Routing table

| From | To | Payload | Trigger |
|---|---|---|---|
| azure-architect / aws-architect | terraform-engineer | #1 | Design complete |
| terraform-engineer | plan-mode-reviewer | #4 | Plan produced, before apply |
| software-engineer | architecture-reviewer | #2 then #3 | Implementation done / fixes applied |
| github-expert | architect agents | OIDC trust policy | Pipeline needs cloud identity |
| any | upstream/human | #5 | Hard blocker or constraint conflict |

## Invariants

- Reviewers are read-only; only engineers modify files.
- Review cycles are bounded: max 2 architecture-review passes, one plan review per plan.
- No payload may contain secrets, credentials, or state file contents.
