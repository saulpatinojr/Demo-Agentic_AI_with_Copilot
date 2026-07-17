# Agentic AI Demo — Wiki

Multi-agent IaC engineering team for **Claude Code** and **GitHub Copilot**.

## Pages

| Page | What's in it |
|---|---|
| [01 — Setup: Claude Code](01-Setup-Claude.md) | Install, drag-and-drop, verification in the Agents center |
| [02 — Setup: GitHub Copilot](02-Setup-Copilot.md) | Install, drag-and-drop, verification in the Agent picker |
| [03 — Test Prompts L1](03-Test-Prompts-L1.md) | Simple single-agent prompts + expected outcomes |
| [04 — Test Prompts L2](04-Test-Prompts-L2.md) | Multi-agent handoff prompts + expected outcomes |
| [05 — Test Prompts L3](05-Test-Prompts-L3.md) | Full-ecosystem tests exercising all agents & skills |
| [06 — Adding Agents & Skills](06-Adding-Agents-and-Skills.md) | Templates and conventions for growing the roster |

## The flow at a glance

```mermaid
flowchart LR
    U[User request] --> O{Orchestrator}
    O -->|Azure design| Arc[azure-architect Arc]
    O -->|AWS design| Out[aws-architect Outpost]
    O -->|App code| Forge[software-engineer Forge]
    O -->|Pipeline| Act[github-expert Actions]
    Arc -->|REQ/TOPOLOGY/CONSTRAINTS| Atlas[terraform-engineer Atlas]
    Out -->|REQ/TOPOLOGY/CONSTRAINTS| Atlas
    Atlas -->|plan + constraints| Sent[plan-mode-reviewer Sentinel]
    Sent -->|approved| Act
    Forge -->|Pass 1 YAML| Comp[architecture-reviewer Compass]
    Comp -->|findings F1..Fn| Forge
    Forge -->|Pass 2 YAML| Comp
    Act -->|OIDC trust policy| Arc
    Act -->|OIDC trust policy| Out
```

**Gates (non-optional):** every Terraform plan passes Sentinel before apply; every implementation passes Compass (max 2 passes); reviewers never write code.
