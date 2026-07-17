---
name: report-issue
description: |
  Use this skill when the user wants to "report a bug", "file an issue", "report an issue", "submit a bug report", or report any problem with this Agentic AI demo (its agents, skills, orchestrator routing, or wiki) to the GitHub repository saulpatinojr/Demo-Agentic_AI_with_Copilot.
allowed-tools: Read, Bash, Glob, Grep, AskUserQuestion, TaskCreate, TaskUpdate, TaskList
user-invocable: true
argument-hint: "[optional: brief description of the bug]"
---

Read and follow all phases in [report-issue-workflow.md](report-issue-workflow.md), located in this skill's folder.

Summary of the workflow:

1. **Identify component** — which agent (azure-architect, aws-architect, terraform-engineer, github-expert, software-engineer, architecture-reviewer, plan-mode-reviewer), skill, orchestrator file, or wiki page the bug concerns, and which platform (Claude Code or GitHub Copilot).
2. **Gather details** — description, repro steps, expected vs actual, optional environment info (OS, Claude Code / Copilot version).
3. **Preview** — show the complete rendered issue to the user and get explicit approval.
4. **Create** — file it with `gh issue create` on `saulpatinojr/Demo-Agentic_AI_with_Copilot`.
5. **Confirm** — return the issue URL.

**Never create the issue without the user approving the full preview. Never include secrets, tenant IDs, tokens, or internal URLs — warn the user the repository is public.**
