# 05 - Test Prompts: L3

L3 tests run the whole ecosystem. Each scenario is still story-driven, but it is broad enough to exercise the orchestrator, the handoffs, the review gates, and the skills together.

## L3.1 - The Grand Tour

Story:
The engineer is launching a small product called OrderTrack. He wants a Python FastAPI app, Azure infrastructure for dev and prod, a simple hub-and-spoke layout, Key Vault, App Service, Azure SQL, CI/CD, and OIDC. He also wants the work to stop for approval before any risky apply or destroy step.

Prompt:
Build OrderTrack with app code, Azure infrastructure, CI/CD, and the proper review gates.

Expected outcome:
- All seven agents can participate in the right order.
- ADRs are written for meaningful decisions.
- Terraform and review gates fire before apply.
- The final summary lists the agents, gates, and artifacts produced.

## L3.2 - Multi-cloud Bake-off

Story:
The engineer has not chosen a cloud yet. He wants both architects to sketch the same small product and then decide which cloud should win before any Terraform is written.

Prompt:
Have the Azure architect and the AWS architect each design the same small OrderTrack environment, then produce a comparison ADR recommending one, and implement Terraform only for the winner.

Expected outcome:
- Arc and Outpost each produce a complete handoff.
- The comparison ADR explains the tradeoffs clearly.
- Atlas implements only the selected design.
- Sentinel gates the plan before apply.

## L3.3 - Brownfield + Failure Path Test

Story:
The engineer inherits a messy legacy Terraform root with hardcoded credentials, public exposure, and no remote backend. He wants it cleaned up safely without destroying stateful resources.

Prompt:
Modernize this legacy Terraform setup into modules, moved blocks, remote state, and a safer CI/CD path. If a change is unsafe, escalate instead of guessing.

Expected outcome:
- Atlas refactors carefully and keeps stateful resources safe.
- Secrets are removed and versions are pinned.
- Sentinel flags any dangerous destroy or replace action.
- Real uncertainty becomes an escalation, not a guess.

## L3.4 - Chaos Routing

Story:
The engineer sends one message with multiple unrelated tasks to see if the orchestrator can split them cleanly and route each piece to the right specialist.

Prompt:
1. What is the AVM rule about output sensitivity? 2. Add a subtract function to calc.py. 3. Is our PR workflow safe against script injection from PR titles? 4. Sketch an AWS DR strategy for OrderTrack with RTO 4h and RPO 15m. Answer all four.

Expected outcome:
- The orchestrator produces an ordered plan and names the right agents or skills.
- Each piece is handled independently.
- Nothing is dropped.
- The final answer summarizes all four tasks.

## Full-coverage checklist

- All seven agents are exercised at least once across L3.1 - L3.4.
- All relevant skills load where needed.
- Reviewers never write code.
- Every apply or destroy step waits for human approval.
- The final summary clearly lists what was produced and what happens next.
