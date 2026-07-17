# 01 — Setup: Claude Code

## Prerequisites

- VS Code with the **Claude Code** extension (or the Claude Code desktop app / CLI), signed in.
- A test folder (empty is fine — e.g. `Test Folder`).
- Optional for L2/L3 infra tests: `terraform`, `tflint`, `tfsec`, `checkov`, `actionlint` on PATH. Prompts still work without them; agents will report which validations they couldn't run.

## Install (drag and drop)

1. Download/clone this repo.
2. Copy the **contents** of the `claude/` folder into the root of your test project:

```
your-project/
├── CLAUDE.md                 ← orchestrator, auto-loaded every session
└── .claude/
    ├── agents/               ← 7 agent definitions
    │   ├── azure-architect.md
    │   ├── aws-architect.md
    │   ├── terraform-engineer.md
    │   ├── github-expert.md
    │   ├── software-engineer.md
    │   ├── architecture-reviewer.md
    │   └── plan-mode-reviewer.md
    └── skills/               ← 5 skills (one folder per skill, SKILL.md inside)
```

> Windows tip: `.claude` is a dot-folder — make sure hidden files are visible in Explorer, or copy via terminal: `Copy-Item -Recurse claude\* your-project\`.

3. Open the project in VS Code and start a **new** Claude session (agents/skills are read at session start).

## Verify in the Agents center

Open **Customizations** in the Claude panel (the same view as in the demo screenshots):

- **Agents** → should list all 7 agents (Project scope, from `.claude/agents/`).
- **Skills** → count should include the 5 project skills (`adr-authoring`, `azure-verified-modules`, `github-actions-security`, `handoff-contracts`, `terraform-validation`).
- **Instructions** → `CLAUDE.md` should show as the project instruction file.

Quick smoke test — ask Claude:

> "List the agents and skills available in this project and what each is for."

Expected: it names all 7 agents with codenames (Arc, Outpost, Atlas, Actions, Forge, Compass, Sentinel) and all 5 skills.

## How routing works

- `CLAUDE.md` is loaded into every session and tells the main model how to classify requests and which subagent to dispatch via the Agent/Task tool.
- Agent `description` frontmatter fields say "Use PROACTIVELY…" so Claude delegates without being told.
- Skills auto-trigger by description match (e.g., editing a workflow file triggers `github-actions-security`), or invoke manually: "use the terraform-validation skill".

## Troubleshooting

| Symptom | Fix |
|---|---|
| Agents don't appear | Confirm files are at `.claude/agents/*.md` with valid YAML frontmatter (`name`, `description`); restart the session. |
| Skills don't appear | Each skill must be a folder containing `SKILL.md` with `name` + `description` frontmatter. |
| Orchestrator ignored | `CLAUDE.md` must be at project root; check the Instructions tab shows it. |
| Agent won't run a tool | Its `tools:` list is intentionally restricted (reviewers are read-only by design). |

Next: [03 — Test Prompts L1](03-Test-Prompts-L1.md)
