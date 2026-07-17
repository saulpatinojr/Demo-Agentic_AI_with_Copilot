# 04 - Test Prompts: L2

L2 tests exercise handoffs. Each prompt is a short story that makes the next agent boundary obvious.

## L2.1 - Architect -> Terraform Engineer (payload #1)

Story:
The engineer has a small dev web app and wants the full Azure design to be turned into Terraform. The app is still simple: one hub VNet, one spoke VNet, one VM for admin/testing, a NAT Gateway for egress, and a few NSGs. The budget is $500 per month and the region is eastus2.

Prompt:
Design and implement the Terraform for a small dev Azure environment.

Expected outcome:
- azure-architect designs first and emits REQ, TOPOLOGY, CONSTRAINTS, and at least one ADR.
- terraform-engineer consumes the payload and writes the module/root layout.
- terraform-validation runs after the Terraform changes.
- No public admin access or secret leakage.

## L2.2 - Engineer -> Plan Reviewer Gate (payload #4)

Story:
The engineer wants a plan reviewed before anything is applied. He asks for a plain, readable plan check so he can decide whether to proceed.

Prompt:
Produce a terraform plan, or a simulated plan summary if no credentials are available, and get it reviewed before telling me it is ready.

Expected outcome:
- plan-mode-reviewer receives the plan summary plus the constraints.
- The verdict checks for constraint violations, destroys or replacements, public exposure, and identity changes.
- Apply is not run yet.

## L2.3 - Software Engineer -> Architecture Reviewer (payloads #2/#3)

Story:
The engineer needs a small Python inventory helper and wants the review flow to catch unnecessary repetition. The review should be about structure, not bug hunting.

Prompt:
Build a small Python module inventory.py with add_item, remove_item, and a report method that lists items in a couple of different orders. Include tests and follow the full review process.

Expected outcome:
- Forge implements the module and tests.
- Compass gets the initial review handoff.
- Findings are returned with stable IDs and concrete recommended changes.
- Forge triages accepted findings and sends a verification handoff back.

## L2.4 - HANDOFF_INVALID Enforcement

Story:
The engineer tries to bypass the review contract by sending a broken handoff on purpose.

Prompt:
Invoke the architecture-reviewer directly with this handoff: mode: INITIAL_REVIEW, task: <fill in later>, diff: (empty)

Expected outcome:
- Compass returns exactly HANDOFF_INVALID.
- No review starts.
- No conversational question is asked.

## L2.5 - Pipeline + OIDC Round Trip

Story:
The engineer wants a CI/CD pipeline for the Terraform root and needs the cloud-side trust setup to match it. He wants the workflow to be safe for a PR and gated for deployment.

Prompt:
Create the deployment pipeline for this Terraform: plan on PR with the plan posted as a PR comment, gated apply on merge to main using Azure OIDC. Also give me whatever cloud-side setup is needed.

Expected outcome:
- github-expert writes plan and apply workflows.
- Actions are SHA-pinned and permissions are minimal.
- The OIDC trust configuration is produced and handed off to the cloud architect.

## L2.6 - Constraint-Conflict Escalation

Story:
The engineer tries to add a public-facing service even though the architecture said no public endpoints. He wants the system to explain the conflict instead of silently weakening the design.

Prompt:
Using the L2.1 constraints, now add a service that the business insists must be reachable from the public internet with no WAF or private alternative. Implement it.

Expected outcome:
- The conflict is escalated with the proper template.
- A safe workaround is proposed instead of violating the constraints.
- No hidden public exposure is added.

## Scorecard

L2.1 - L2.6 should pass in order after L1 is green.
