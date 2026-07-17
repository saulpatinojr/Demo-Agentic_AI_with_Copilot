# Report Issue — Workflow

File a bug report on the [saulpatinojr/Demo-Agentic_AI_with_Copilot](https://github.com/saulpatinojr/Demo-Agentic_AI_with_Copilot) GitHub repository. This workflow gathers the required information, presents a full preview, and creates the issue on the user's behalf.

**Initial request:** $ARGUMENTS

> **WARNING:** Before proceeding, inform the user: "This will create an issue on the **public** GitHub repository saulpatinojr/Demo-Agentic_AI_with_Copilot. **Do not include any sensitive information** such as credentials, internal URLs, tenant/subscription IDs, customer data, or proprietary code."

---

## Phase 1: Identify Component & Platform

**Goal**: Determine what the bug is about.

**Actions**:

1. Create a task list with all 5 phases (see Progress Tracking below).
2. **Auto-detect candidates**: Glob `.claude/agents/*.md`, `.github/agents/*.agent.md`, and `*/skills/*/SKILL.md` in the workspace to list the installed agents and skills.
3. Use `AskUserQuestion`:
   - **"Which component has the bug?"** (header: "Component") — options: "An agent", "A skill", "Orchestrator/routing", "Wiki/docs".
   - **"Which platform were you running?"** (header: "Platform") — options: "Claude Code", "GitHub Copilot", "Both/unsure".
4. If "An agent" or "A skill", follow up with the detected names as options.

**Output**: Component, name, and platform confirmed.

---

## Phase 2: Gather Bug Details

**Goal**: Collect all required and optional fields.

**Actions**:

1. If `$ARGUMENTS` contains a description, extract as much as possible to pre-fill fields; still confirm with the user.
2. Use `AskUserQuestion` (free-text answers come via "Other"):
   - **"Please describe the bug clearly and concisely."** (header: "Bug") — hint options: "Agent routed incorrectly", "Skill did not trigger", "Handoff payload invalid", "Wrong/unsafe output".
   - **"What steps reproduce the bug?"** (header: "Repro steps") — hint options: "Ran an L1 test prompt", "Ran an L2/L3 test prompt", "Custom prompt".
   - **"What did you expect / what actually happened?"** (headers: "Expected" / "Actual").
3. Environment details — ask: **"Include environment details (OS, Claude Code/Copilot version, terraform/tflint versions) to help debug?"** — "Yes, auto-collect" (recommended) / "No, skip".
   - If yes, collect via shell: OS name/version, `claude --version` or Copilot extension version, `terraform version`, `git --version`. Present the collected values before including them.

**Output**: All fields collected.

---

## Phase 3: Preview

**Goal**: User sees exactly what will be filed.

Render the complete issue and show it verbatim:

```markdown
**Title**: [<component>/<name>] <concise bug summary>

## Component
- Component: <agent|skill|orchestrator|wiki> — <name>
- Platform: <Claude Code | GitHub Copilot | Both>

## Description
<description>

## Steps to Reproduce
1. <steps>

## Expected Behavior
<expected>

## Actual Behavior
<actual>

## Environment (optional)
- OS: <...>
- Claude Code / Copilot: <...>
- terraform / tooling: <...>
```

Use `AskUserQuestion`: **"Create this issue?"** — "Yes, file it" / "Edit something" / "Cancel". Loop on edits until approved. **Never file without explicit approval.**

---

## Phase 4: Create the Issue

**Actions**:

1. Verify auth: `gh auth status`. If not authenticated, instruct the user to run `gh auth login` and stop.
2. Write the body to a temp file, then:
   `gh issue create --repo saulpatinojr/Demo-Agentic_AI_with_Copilot --title "<title>" --body-file <tmpfile> --label bug`
   (If the `bug` label doesn't exist, retry without `--label`.)
3. Capture the returned URL.

---

## Phase 5: Confirm

Report to the user: the issue URL, title, and a one-line recap. Mark all tasks complete.

---

## Progress Tracking

| # | Phase | Status |
|---|---|---|
| 1 | Identify component & platform | ☐ |
| 2 | Gather bug details | ☐ |
| 3 | Preview & approval | ☐ |
| 4 | Create issue | ☐ |
| 5 | Confirm | ☐ |
