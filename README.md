# Demo: Agentic AI with Claude & GitHub Copilot

A drag-and-drop demo of a **multi-agent IaC engineering team** that runs identically on **Claude Code** and **GitHub Copilot** inside VS Code / the Agents center. Seven specialized agents, eleven skills, and an orchestrator layer with typed handoff contracts — designed so the flow routes correctly every time.

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

**Skills:** `handoff-contracts` (the glue), `terraform-validation`, `github-actions-security`, `azure-verified-modules`, `adr-authoring`, plus the Azure ops pack: `azure-cloud-migrate`, `azure-compliance`, `azure-cost`, `azure-deploy`, `azure-resource-visualizer`, and `report-issue` for filing bugs against this demo.

## Repository layout

```
├── claude/                     ← drag the CONTENTS into your project root for Claude Code
│   ├── CLAUDE.md               ← orchestrator (auto-loaded by Claude Code)
│   └── .claude/
│       ├── agents/             ← 7 agents
│       └── skills/             ← 11 skills
└── copilot/                    ← drag the CONTENTS into your project root for GitHub Copilot
    └── .github/
        ├── copilot-instructions.md   ← orchestrator (auto-loaded by Copilot)
        ├── agents/             ← 7 agents (.agent.md)
        └── skills/             ← 11 skills
```

Full documentation lives in the **[GitHub Wiki](https://github.com/saulpatinojr/Demo-Agentic_AI_with_Copilot/wiki)** — setup guides, test prompts with expected outcomes, and the extension guide.

## Where to go

1. **Setup** — [Setup: Claude Code](https://github.com/saulpatinojr/Demo-Agentic_AI_with_Copilot/wiki/01-Setup-Claude) or [Setup: GitHub Copilot](https://github.com/saulpatinojr/Demo-Agentic_AI_with_Copilot/wiki/02-Setup-Copilot)
2. **Test it** — [L1 simple](https://github.com/saulpatinojr/Demo-Agentic_AI_with_Copilot/wiki/03-Test-Prompts-L1) → [L2 advanced](https://github.com/saulpatinojr/Demo-Agentic_AI_with_Copilot/wiki/04-Test-Prompts-L2) → [L3 full-ecosystem](https://github.com/saulpatinojr/Demo-Agentic_AI_with_Copilot/wiki/05-Test-Prompts-L3)
3. **Extend it** — [Adding Agents & Skills](https://github.com/saulpatinojr/Demo-Agentic_AI_with_Copilot/wiki/06-Adding-Agents-and-Skills)

## Quick start (60 seconds)

Clone the repo, then copy the bundle **contents** (not the folder itself) into your test project:

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
1. Open your project in VS Code with Claude Code (or Copilot) and start a **new** session — the Agents center should show 7 agents and 11 skills.
2. Paste an L1 prompt from the [wiki](https://github.com/saulpatinojr/Demo-Agentic_AI_with_Copilot/wiki/03-Test-Prompts-L1), e.g. *"Design a small Azure landing zone for a dev web app"* — and watch it route to Arc.

> Heads-up: `.claude`/`.github` are dot-folders — if you drag-and-drop in Explorer/Finder instead of using the commands above, make sure hidden files are visible so they actually get copied.
