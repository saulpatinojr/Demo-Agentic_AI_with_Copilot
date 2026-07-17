# 03 - Test Prompts: L1

L1 tests exercise one agent or one skill at a time. Each prompt is written as a small story so the expected behavior is easy to check.

## L1.1 - Azure Architect (Arc)

Story:
The engineer has a small dev web app to place in Azure. He wants a simple hub-and-spoke layout with one hub VNet, one spoke VNet, one Linux VM in the spoke for admin and testing, a NAT Gateway for egress, and a few NSGs or ASGs to keep traffic controlled. He has a budget of $500 per month, wants eastus2, and does not need on-prem connectivity.

Prompt:
Design a small Azure landing zone for this dev app.

Expected outcome:
- Routed to azure-architect.
- Output includes REQ, TOPOLOGY, CONSTRAINTS, and at least one ADR.
- The design stays small and cost-aware.
- Assumptions are stated clearly, including any SLA, RTO, or RPO assumptions.
- No public RDP or SSH is introduced.

## L1.2 - AWS Architect (Outpost)

Story:
The engineer is doing the same small dev app in AWS. He wants a compact hub-and-spoke style design with one shared network layer, one workload VPC, one EC2 VM for admin and testing, a NAT Gateway for egress, and a few security groups or network rules to control access. He wants us-east-1, a $500 per month budget, and no on-prem connectivity.

Prompt:
Design a small AWS landing zone for this dev app.

Expected outcome:
- Routed to aws-architect.
- Output includes REQ, TOPOLOGY, CONSTRAINTS, and at least one ADR.
- The design stays small and cost-aware.
- Assumptions are stated clearly, including any SLA, RTO, or RPO assumptions.
- No public admin access is introduced.

## L1.3 - Terraform Engineer (Atlas)

Story:
The engineer is turning the network sketch into reusable Terraform. He wants a provider-agnostic module skeleton called network with typed inputs, outputs, and a README so the module can be reused later.

Prompt:
Write a provider-agnostic Terraform module skeleton called network with typed variables for environment, name, tags, and cidr_block, plus outputs and a README.

Expected outcome:
- Routed to terraform-engineer.
- Files created for a small module layout such as main.tf, variables.tf, outputs.tf, versions.tf, and README.md.
- Variables are typed, described, and validated.
- No provider-specific resources are hardcoded in the module interface.

## L1.4 - GitHub Expert (Actions)

Story:
The engineer wants a clean pull-request check for the Terraform module before merging it. He asks for a small CI workflow that validates formatting and static checks without overcomplicating the pipeline.

Prompt:
Create a CI workflow that runs terraform fmt, terraform validate, and tflint on pull requests to main.

Expected outcome:
- Routed to github-expert.
- Workflow files are created under .github/workflows.
- Actions are pinned to SHAs and permissions are minimal.
- The workflow includes a concurrency group and a clear security posture.

## L1.5 - Software Engineer (Forge)

Story:
The engineer needs a tiny helper used by the demo app and wants to keep the change trivial. He asks for one small function and a test so the review flow can be checked without turning the task into a large feature.

Prompt:
Create a Python file calc.py with an add(a, b) function and a test.

Expected outcome:
- Routed to software-engineer.
- The implementation is small and test-backed.
- If the change is trivial, the review skip logic is stated explicitly.

## L1.6 - Architecture Reviewer (Compass)

Story:
The engineer wants a quick architectural review of the tiny helper before opening a PR, and he mistakenly asks the reviewer to fix anything it finds.

Prompt:
Ask the architecture-reviewer to review calc.py and also to fix anything it finds.

Expected outcome:
- The reviewer runs in read-only mode.
- Findings are suggestions with stable IDs.
- The reviewer refuses to edit files and sends fixes back to the software-engineer.

## L1.7 - Plan Mode Reviewer (Sentinel)

Story:
The engineer has a risky plan for a new endpoint and wants a safety gate before implementation. He wants the reviewer to point out the tradeoffs instead of rubber-stamping the idea.

Prompt:
Review this plan before I implement: Add a /users REST endpoint reading directly from the DB in the route handler, no tests, ship today.

Expected outcome:
- The reviewer returns numbered issues with lettered options.
- Tradeoffs are explained clearly.
- A recommendation is given, and the user is asked to choose.

## L1.8 - Skill Trigger Check (AVM)

Story:
The engineer is writing an Azure Terraform module and needs to know the provider version constraints for AVM compliance.

Prompt:
I am writing an Azure Terraform module that needs AVM certification. What provider version constraints must I use?

Expected outcome:
- azure-verified-modules loads.
- The answer uses the AVM constraints from the skill guidance.

## L1.9 - Azure Cost Skill Trigger

Story:
The engineer wants a quick view of spend before he keeps building. He needs to know where the waste is and what to cut first.

Prompt:
How much am I spending on Azure this month, and where can I cut waste?

Expected outcome:
- azure-cost loads.
- The workflow follows cost query and cost optimization guidance.
- If Azure access is missing, the answer explains which commands would be used.

## L1.10 - Azure Compliance Skill Trigger

Story:
The engineer wants a compliance and security audit before release. He also wants an expiration check for secrets and certificates in Key Vault.

Prompt:
Run a compliance and security audit of my Azure subscription, including Key Vault secrets that are about to expire.

Expected outcome:
- azure-compliance loads.
- The answer separates scan, findings, and remediation.
- The skill does not route this to the cost workflow.

## L1.11 - Azure Cloud Migrate Skill Trigger

Story:
The engineer is moving one small AWS serverless workload into Azure and wants an assessment before touching code.

Prompt:
Assess migrating our AWS Lambda functions to Azure Functions.

Expected outcome:
- azure-cloud-migrate loads.
- An assessment report is produced before code migration.
- The workflow hands the target design to azure-architect when needed.

## L1.12 - Draw.io Diagrams

Story:
The engineer wants a clean architecture diagram for the demo so the layout is easy to explain during a walkthrough.

Prompt:
Create a draw.io diagram of a small Azure hub-spoke network with one hub, one spoke, one VM, one egress gateway, and a few network security resources. Export a PNG.

Expected outcome:
- drawio-skill loads.
- Official vendor icons are used.
- The diagram is clean, readable, and exported correctly.

## L1.13 - Azure Deploy Skill Boundary Check

Story:
The engineer asks for a brand-new Azure app and deployment in one shot, but this skill should only be used after the app and infrastructure already exist.

Prompt:
Create a new FastAPI app and deploy it to Azure.

Expected outcome:
- azure-deploy does not trigger for the create-and-deploy request.
- The orchestrator routes creation to the app or architecture agents first.
- Deployment happens later, after validation and approval.

## L1.14 - Report Issue Skill

Story:
The engineer sees a bug in the Terraform flow and wants to file it as a GitHub issue in the demo repository.

Prompt:
I want to report a bug: the terraform-engineer agent skipped validation.

Expected outcome:
- report-issue loads.
- The workflow gathers the details, shows a preview, and waits for approval before creating the issue.
- The repository being public is called out clearly.

## Scorecard

L1.1 - L1.14 should pass one at a time in a fresh session.
