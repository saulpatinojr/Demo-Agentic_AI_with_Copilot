# Official Vendor Icons & Professional Presentation Standards

Read this for EVERY cloud/vendor architecture diagram (Azure, AWS, GCP, Kubernetes, Cisco, etc.). These standards exist because diagrams from this ecosystem are shown to architecture review boards and executives — a generic gray rectangle labeled "Lambda" reads as a draft; the official orange Lambda icon reads as a deliverable.

## 1. Official vendor icons are mandatory

Never draw a vendor service as a plain rectangle, ellipse, or hand-invented style string. Always resolve the official shape:

```bash
python3 <this-skill-dir>/scripts/shapesearch.py "azure virtual network"
python3 <this-skill-dir>/scripts/shapesearch.py "aws lambda"
```

The index covers 10,000+ official stencils. Rules for choosing among the results:

| Vendor | Use this family | Avoid | Why |
|---|---|---|---|
| **Azure** | `img/lib/azure2/...` image styles (current Microsoft icon set) | `mxgraph.azure.*` (legacy flat set) | azure2 is the set Microsoft ships in its own architecture docs |
| **AWS** | `mxgraph.aws4.resourceIcon` + `resIcon=mxgraph.aws4.<service>` | `mxgraph.aws3.*`, `mxgraph.aws3d.*` | aws4 is the current AWS Architecture Icons standard |
| GCP | `mxgraph.gcp2.*` | gcpIcons legacy | current Google set |
| Kubernetes | `mxgraph.kubernetes.*` | generic hexagons | official CNCF shapes |

**Never mix icon generations in one diagram** (e.g., one aws3 icon among aws4 icons) — mismatched visual weight is instantly noticeable.

**AI/LLM brands** (OpenAI, Claude, Gemini…): draw.io has no built-ins — use `scripts/aiicons.py "<brand>"`.

## 2. Icon sizing — uniform within a family

Icons must be the SAME size across the diagram, at the family's native aspect:

- **aws4 resourceIcon**: 78×78 for every service icon. AWS group containers use the aws4 group styles.
- **azure2 image shapes**: keep the native aspect from shapesearch output (most are ~64–68 wide); scale all to a uniform height of **64** unless the diagram is very dense (then 48).
- Vendor logos as area labels (e.g., an Azure logo marking a subscription boundary): 32–40 px, top-left corner of the container, never centered behind content.

## 3. Spacing & alignment (the difference between draft and deliverable)

All coordinates on the **10px grid**. Then:

| Measurement | Minimum | Preferred |
|---|---|---|
| Gap between sibling icons (same tier) | 60 | **100** |
| Gap between tiers/layers (vertical flow) | 80 | **120** |
| Container internal padding (content to container border) | 30 | **40** |
| Gap between containers | 60 | **80** |
| Container title strip height | 30 | 30 |
| Edge label to nearest other element | 10 | 20 |

- **Align centers, not corners**: icons in a row share the same center-Y; icons in a column share center-X. Mixed icon heights (azure2 shapes vary) must be center-aligned, never top-aligned.
- **Distribute evenly**: 3+ siblings in a row get identical center-to-center spacing (e.g., centers at x=200, 360, 520 — not 200, 350, 540).
- **Symmetry**: when one parent fans out to N children, center the parent above/beside the group of children.

## 4. Labels & typography

- Icon labels: `verticalLabelPosition=bottom;verticalAlign=top` (below the icon — where both AWS and Azure put them in official docs), `fontSize=12`.
- Container titles: bold, `fontSize=14`, in the title strip, left-aligned with 10px inset.
- Diagram title block: every deliverable diagram gets a title at top-left — diagram name, `fontSize=20;fontStyle=1`, plus a one-line subtitle (`fontSize=11;fontColor=#666666`) with date and environment.
- Edge labels: `fontSize=11;labelBackgroundColor=#ffffff` always — bare text crossing a line is unreadable.
- No label may overflow its shape. If it does, widen the shape to the next 10px multiple — don't shrink the font below 11.

## 5. Containers & grouping (cloud semantics must be correct)

Model the real boundary hierarchy with nested containers, drawn as rounded rectangles with subtle fills:

- **Azure**: Subscription → Resource Group → VNet → Subnet. Suggested fills (light, `strokeColor` a darker shade of the same hue): subscription `#F8F8F8/#666666`, VNet `#E6F2FA/#0078D4` (Azure blue), subnet `#FFFFFF/#7FBA00` dashed.
- **AWS**: Account → Region (dashed) → VPC `#E8F4E8/#248814` → Availability Zone (dashed, no fill) → Subnet (public `#E9F7EF`, private `#FDEBD0` — the AWS convention).
- Containers are `container=1;collapsible=0`, children have `parent` set to the container id with **relative child coordinates** (see `xml-authoring.md`).
- Every container gets a label; unlabeled boxes are noise.

## 6. Edges

- `edgeStyle=orthogonalEdgeStyle;rounded=1;jettySize=auto;strokeWidth=1.5` for architecture flows.
- One color for the primary flow (dark gray `#555555`); reserve colored edges for meaning (e.g., `#B85450` dashed = private/ExpressRoute link, `#82B366` = data replication) and add a small legend when you use more than one edge color.
- Distribute connection points (`exitX/entryX`) so parallel edges never stack; route around shapes with waypoints — an edge crossing an unrelated icon fails review.
- Arrows point in the direction of the dependency/flow, consistently — never mixed metaphors in one diagram.

## 7. Pre-export quality checklist

Run through this before every export (in addition to `scripts/validate.py`):

- [ ] Every vendor service uses its official current-generation icon (via shapesearch — no invented styles)
- [ ] One icon family per vendor, uniform sizes, center-aligned rows/columns
- [ ] All gaps meet the minimums in §3; siblings evenly distributed
- [ ] Boundary hierarchy correct (subscription/account → network → subnet) and labeled
- [ ] Title block present; every edge label has a white background
- [ ] No edge crosses an icon; no two edges stack; colored edges have a legend
- [ ] `validate.py` clean; then vision self-check per SKILL.md step 5
