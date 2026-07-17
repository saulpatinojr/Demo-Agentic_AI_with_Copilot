# Demo: Agentic AI with Claude & GitHub Copilot

A drag-and-drop demo of a **multi-agent IaC engineering team** that runs identically on **Claude Code** and **GitHub Copilot** inside VS Code / the Agents center. Seven specialized agents, five skills, and an orchestrator layer with typed handoff contracts — designed so the flow routes correctly every time.

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

**Skills:** `handoff-contracts` (the glue), `terraform-validation`, `github-actions-security`, `azure-verified-modules`, `adr-authoring`.

## Repository layout

```
├── claude/                     ← drag the CONTENTS into your project root for Claude Code
│   ├── CLAUDE.md               ← orchestrator (auto-loaded by Claude Code)
│   └── .claude/
│       ├── agents/             ← 7 agents
│       └── skills/             ← 5 skills
├── copilot/                    ← drag the CONTENTS into your project root for GitHub Copilot
│   └── .github/
│       ├── copilot-instructions.md   ← orchestrator (auto-loaded by Copilot)
│       ├── agents/             ← 7 agents (.agent.md)
│       └── skills/             ← 5 skills
└── wiki/                       ← full setup guide + L1/L2/L3 test prompts & expected outcomes
```

## Where to go

1. **Setup** — [wiki/01-Setup-Claude.md](wiki/01-Setup-Claude.md) or [wiki/02-Setup-Copilot.md](wiki/02-Setup-Copilot.md)
2. **Test it** — [L1 simple](wiki/03-Test-Prompts-L1.md) → [L2 advanced](wiki/04-Test-Prompts-L2.md) → [L3 full-ecosystem](wiki/05-Test-Prompts-L3.md)
3. **Extend it** — [wiki/06-Adding-Agents-and-Skills.md](wiki/06-Adding-Agents-and-Skills.md)

> The `wiki/` folder is also written to be copy-pasted directly into this repo's GitHub Wiki (each file = one page).

## Quick start (60 seconds)

1. Copy the contents of `claude/` (or `copilot/`) into any test folder.
2. Open the folder in VS Code with Claude Code (or Copilot) — the Agents center should show 7 agents and 5 skills.
3. Paste an L1 prompt from the wiki, e.g. *"Design a small Azure landing zone for a dev web app"* — and watch it route to Arc.
