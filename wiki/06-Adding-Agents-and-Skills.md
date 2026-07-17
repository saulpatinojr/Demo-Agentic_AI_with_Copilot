# 06 — Adding Agents & Skills

Grow the roster without breaking the routing. Every addition touches **four places**: the agent/skill file in each bundle, the orchestrator roster, and (ideally) a test prompt.

## Adding a skill

Skills are identical on both platforms — one folder, one `SKILL.md`.

1. Create the folder in both bundles:
   - `claude/.claude/skills/<skill-name>/SKILL.md`
   - `copilot/.github/skills/<skill-name>/SKILL.md`
2. Use this template:

```markdown
---
name: <kebab-case-name>
description: <One or two sentences. CRITICAL: this is the trigger — describe WHEN to use it ("Use whenever...") and what it covers, with the keywords a request would contain.>
---

# <Title>

<Procedural content: checklists, command sequences, templates, rules.
Keep it under ~500 lines; link out or add reference files in the same
folder for anything longer.>
```

3. Rules of thumb:
   - The `description` does 90% of the work — write it for the router, not the reader.
   - Skills are **procedures and reference**, not personas. If it needs its own tools or judgment, it's an agent.
   - One skill = one job. Split "terraform-and-security" into two.
4. If an agent should always use it, name the skill in that agent's instructions (as terraform-engineer names `terraform-validation`).
5. Add one L1 trigger-test prompt to the wiki.

**Skill ideas queued for this demo:** `finops-cost-estimation` (Infracost), `terraform-state-surgery`, `aws-well-architected-review`, `mermaid-topology-diagrams`.

## Adding an agent

### 1. Claude bundle — `claude/.claude/agents/<name>.md`

```markdown
---
name: <kebab-case-name>
description: "<Role> (Codename: <Name>). Use PROACTIVELY for <triggers>. <What it produces and who it hands off to.>"
tools: Read, Grep, Glob, Bash, WebFetch, WebSearch   # add Write, Edit ONLY if it modifies files
---

# <Agent> instructions

<Full persona: mission, operating rules, output format, handoff contracts.>
```

### 2. Copilot bundle — `copilot/.github/agents/<name>.agent.md`

```markdown
---
name: <same-name>
description: "<same description>"
tools: ['view', 'grep', 'glob', 'bash', 'read_bash', 'web_fetch', 'web_search']  # add 'create', 'edit' only for writers
agents: []            # subagents it may invoke
handoffs:
  - label: <button text>
    agent: <target-agent>
    prompt: "<pre-filled handoff message referencing the payload contract>"
    send: false
---

<Identical instruction body as the Claude version.>
```

### 3. Register with the orchestrator (both files!)

In `claude/CLAUDE.md` **and** `copilot/.github/copilot-instructions.md`:
- Add a row to **The Roster** table (with codename and "Writes files?").
- Add/extend a routing rule or standard flow so the orchestrator knows *when* to dispatch it.

### 4. Wire the handoffs

- If it consumes or produces a payload, add the schema to the `handoff-contracts` skill (both copies) and a row to its routing table.
- If reviewers must gate it, say so explicitly ("X's output goes to Sentinel before Y").

### 5. Design rules that keep the ecosystem 110% reliable

| Rule | Why |
|---|---|
| Reviewers get NO write tools | Separation of powers is enforced by tool grants, not trust |
| Every loop is bounded (state the max pass count) | Prevents infinite agent ping-pong |
| Payloads are typed; receivers reject invalid ones | `HANDOFF_INVALID` beats hallucinated context |
| One codename per agent, used everywhere | Handoffs reference agents unambiguously |
| Body content identical across both bundles | One behavior, two runtimes |
| Every new agent ships with at least one L1 test | Untested routing is broken routing |

**Agent ideas queued for this demo (referenced by existing agents):** `finops-practitioner` (Infracost), `gcp-architect`, `kubernetes-engineer` (Helm/ARC), `vmware-administrator` (vSphere), `documentation-expert` (Sphinx/Mermaid).
