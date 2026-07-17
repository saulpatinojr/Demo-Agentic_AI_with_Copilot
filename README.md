# Demo: Agentic AI with Claude & GitHub Copilot

A drag-and-drop demo of a **multi-agent IaC engineering team** that runs identically on **Claude Code** and **GitHub Copilot** inside VS Code / the Agents center. Seven specialized agents, twelve skills, and an orchestrator layer with typed handoff contracts — designed so the flow routes correctly every time.

## What this demo is trying to achieve

Prove that a single, well-structured set of agent definitions can:

1. **Route work deterministically** — an orchestrator classifies each request (design / IaC / pipeline / app code / review) and dispatches to the right specialist in the right order.
2. **Enforce separation of powers** — engineers write, reviewers only read; every implementation passes a bounded 2-pass architecture review, and every Terraform plan passes a constraint gate before apply.
3. **Hand off with contracts, not vibes** — agents exchange typed payloads (REQ/TOPOLOGY/CONSTRAINTS, review YAML, plan-review YAML). Malformed handoffs are rejected with `HANDOFF_INVALID` instead of guessed at.
4. **Work on both platforms** — the same roster and rules, packaged once for Claude Code and once for GitHub Copilot.

## The team

| Agent | Codename | Role |
|---|---|---|
| azure-architect | **Arc** | Azure/hybrid topology design (WAF/CAF, Zero Trust) |
| aws-architect | **Outpost** | AWS topology design (Well-Architected, Control Tower) |
| terraform-engineer | **Atlas** | Cloud-agnostic Terraform authoring + validation |
| github-expert | **Actions** | GitHub Actions pipelines, OIDC, repo governance |
| software-engineer | **Forge** | Production application code, autonomous implementation |
| architecture-reviewer | **Compass** | Read-only design/reuse review (2-pass cycle) |
| plan-mode-reviewer | **Sentinel** | Read-only plan review & constraint gate |

**Skills:** `handoff-contracts` (the glue), `drawio-skill` (the official diagrams skill — draw.io with the official Azure/AWS icon sets and enforced presentation standards), `terraform-validation`, `github-actions-security`, `azure-verified-modules`, `adr-authoring`, the Azure ops pack (`azure-cloud-migrate`, `azure-compliance`, `azure-cost`, `azure-deploy`), and `report-issue` for filing bugs against this demo.

## Release 0.5

This release simplifies the test prompts into smaller, story-driven scenarios so they can be run one by one without carrying too much context.

- L1 prompts are now intentionally small and focused on a single agent or skill.
- L2 prompts keep the handoff flow but reduce the amount of infrastructure at each step.
- L3 prompts still exercise the full ecosystem, but each scenario is easier to follow and explain.
- The wiki content now has a local markdown source mirror in `wiki/` so the prompt set is easier to maintain.

## Cheat sheet

### Which agent to use

| If the request is about... | Use this agent | What it does |
|---|---|---|
| Azure architecture or landing zones | azure-architect / Arc | Designs Azure or hybrid topology and produces REQ/TOPOLOGY/CONSTRAINTS/ADR handoffs |
| AWS architecture or landing zones | aws-architect / Outpost | Designs AWS or hybrid topology and produces the same handoffs for AWS |
| Terraform code or modules | terraform-engineer / Atlas | Writes, refactors, validates, and operationalizes Terraform |
| GitHub Actions or repo governance | github-expert / Actions | Builds secure workflows, OIDC, branch protection, and repo automation |
| Application code or feature work | software-engineer / Forge | Implements the app feature end-to-end |
| Design or reuse review after implementation | architecture-reviewer / Compass | Read-only review focused on architecture, reuse, and complexity |
| Terraform plan safety review | plan-mode-reviewer / Sentinel | Read-only plan gate before apply |

### Which skill to load

| If you need... | Load this skill | What it is for |
|---|---|---|
| Agent-to-agent payload checks | handoff-contracts | Validates REQ, TOPOLOGY, CONSTRAINTS, and review handoffs |
| Terraform safety checks | terraform-validation | Runs fmt, validate, tests, linting, and security scans |
| Workflow security review | github-actions-security | Hardens workflow YAML and OIDC usage |
| Azure module compliance | azure-verified-modules | Checks Azure Terraform modules against AVM rules |
| Decision records | adr-authoring | Writes ADRs for non-obvious design choices |
| Azure migration | azure-cloud-migrate | Assesses and migrates workloads to Azure |
| Azure compliance audits | azure-compliance | Runs azqr and Key Vault expiration checks |
| Azure cost analysis | azure-cost | Queries, forecasts, and optimizes spend |
| Prepared Azure deployment | azure-deploy | Executes an already-validated Azure deployment |
| Azure resource diagrams | azure-resource-visualizer | Builds Mermaid diagrams from live Azure resources |
| Polished architecture diagrams | drawio-skill | Creates draw.io diagrams with official vendor icons |
| Filing a repo bug | report-issue | Creates an issue for this demo after preview and approval |

### Fast routing rules

1. Design before code: architecture requests go to Arc or Outpost first.
2. Code before review: Forge or Atlas finish work, then reviewers gate it.
3. Handoff with contracts: never pass free-form design between agents when a typed payload is required.
4. Use the review skills before apply or merge: Sentinel for Terraform plans, Compass for implementation review.

## Repository layout

```
├── claude/                     ← drag the CONTENTS into your project root for Claude Code
│   ├── CLAUDE.md               ← orchestrator (auto-loaded by Claude Code)
│   └── .claude/
│       ├── agents/             ← 7 agents
│       └── skills/             ← 12 skills
└── copilot/                    ← drag the CONTENTS into your project root for GitHub Copilot
    └── .github/
        ├── copilot-instructions.md   ← orchestrator (auto-loaded by Copilot)
        ├── agents/             ← 7 agents (.agent.md)
        └── skills/             ← 12 skills
```

Full documentation lives in the **[GitHub Wiki](https://github.com/saulpatinojr/Demo-Agentic_AI_with_Copilot/wiki)** — setup guides, test prompts with expected outcomes, and the extension guide.

## Where to go

1. **Setup** — [Setup: Claude Code](https://github.com/saulpatinojr/Demo-Agentic_AI_with_Copilot/wiki/01-Setup-Claude) or [Setup: GitHub Copilot](https://github.com/saulpatinojr/Demo-Agentic_AI_with_Copilot/wiki/02-Setup-Copilot)
2. **Test it** — [L1 simple](https://github.com/saulpatinojr/Demo-Agentic_AI_with_Copilot/wiki/03-Test-Prompts-L1) → [L2 advanced](https://github.com/saulpatinojr/Demo-Agentic_AI_with_Copilot/wiki/04-Test-Prompts-L2) → [L3 full-ecosystem](https://github.com/saulpatinojr/Demo-Agentic_AI_with_Copilot/wiki/05-Test-Prompts-L3)
3. **Extend it** — [Adding Agents & Skills](https://github.com/saulpatinojr/Demo-Agentic_AI_with_Copilot/wiki/06-Adding-Agents-and-Skills)

## Quick start (60 seconds)

Create a **new, blank folder** for your test project (do not reuse an existing folder, and do not open the cloned repo itself as the project). Clone the repo, then copy the bundle **contents** (not the folder itself) into it:

**Windows (PowerShell):**
```powershell
git clone https://github.com/saulpatinojr/Demo-Agentic_AI_with_Copilot.git
Copy-Item -Recurse -Force Demo-Agentic_AI_with_Copilot\claude\* your-project\     # Claude Code
# or
Copy-Item -Recurse -Force Demo-Agentic_AI_with_Copilot\copilot\* your-project\   # GitHub Copilot
```

**macOS/Linux:**
```bash
git clone https://github.com/saulpatinojr/Demo-Agentic_AI_with_Copilot.git
cp -r Demo-Agentic_AI_with_Copilot/claude/. your-project/     # Claude Code
# or
cp -r Demo-Agentic_AI_with_Copilot/copilot/. your-project/    # GitHub Copilot
```

Then:
1. Open your project in VS Code with Claude Code (or Copilot) and start a **new** session — the Agents center should show 7 agents and 12 skills.
2. Paste an L1 prompt from the [wiki](https://github.com/saulpatinojr/Demo-Agentic_AI_with_Copilot/wiki/03-Test-Prompts-L1), e.g. *"Design a small Azure landing zone for a dev web app"* — and watch it route to Arc.

> Heads-up: `.claude`/`.github` are dot-folders — if you drag-and-drop in Explorer/Finder instead of using the commands above, make sure hidden files are visible so they actually get copied.
>
> **Verify the nesting:** the dot-folder must sit at the project's TOP level (`your-project/.claude/` or `your-project/.github/`). If you see `your-project/claude/` or `your-project/copilot/`, you copied the folder instead of its contents and nothing will register — move the contents up one level.
