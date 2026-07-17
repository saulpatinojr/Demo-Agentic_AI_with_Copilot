---
name: adr-authoring
description: Architecture Decision Record (ADR) template and authoring rules. Use whenever an architect or engineer agent makes a non-obvious design choice that downstream agents or humans must understand — cloud service selection, topology changes, tool choices, constraint trade-offs.
---

# ADR Authoring

Write one ADR per significant decision. Store under `ADRs/` (or `docs/adr/` if the repo already uses it), numbered sequentially: `NNNN-short-kebab-title.md`.

## Template

```markdown
# NNNN — <Decision title>

- **Status**: Proposed | Accepted | Superseded by NNNN
- **Date**: YYYY-MM-DD
- **Deciders**: <agent codename(s) and/or humans>
- **Technical depth**: L100 (exec) | L200 (design) | L300 (implementation)

## Context and Problem Statement

<2-5 sentences: the forces at play, the requirement (REQ reference), the constraint that makes this a real decision.>

## Considered Options

1. <Option A>
2. <Option B>
3. <Option C — include "do nothing" when relevant>

## Decision

Chosen option: **<Option>**, because <primary rationale in one sentence>.

## Consequences

- **Positive**: <cost, operational, security, simplicity wins>
- **Negative**: <lock-in, operational overhead, risks accepted>
- **Follow-ups**: <refactors, revisits, expiry conditions>
```

## Rules

1. One decision per ADR — split compound decisions.
2. Options must be real: at least two genuinely viable alternatives with honest trade-offs (cost, ops overhead, vendor lock-in).
3. Write the Context so a reader in 6 months understands *why* without this conversation.
4. Never edit an Accepted ADR's decision — supersede it with a new one.
5. Reference upstream payloads (REQ/TOPOLOGY/CONSTRAINTS ids) so decisions trace to requirements.
6. Depth ladder: an L300 ADR includes concrete SKUs/instance types, versions, and limits; an L100 ADR never does.
