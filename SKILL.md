---
name: mermaid-ops-arch
description: Create operations process maps and system diagrams. Use when asked to "map a process", "create a diagram", "document a system", "visualize a workflow", or "create an operations diagram".
disable-model-invocation: true
---

# Mermaid Architect - Operations Process Mapping Skill

Operations process mapping and system diagram skill with specialized guides and on-demand loading.

## Routing

- workflow, process, swimlane, ops map, cross-functional → `$CLAUDE_SKILL_DIR/references/guides/diagrams/activity-diagrams.md`
- deployment, infrastructure, cloud, k8s, serverless → `$CLAUDE_SKILL_DIR/references/guides/diagrams/deployment-diagrams.md`
- architecture, system design, components → `$CLAUDE_SKILL_DIR/references/guides/diagrams/architecture-diagrams.md`
- API, sequence, service interactions → `$CLAUDE_SKILL_DIR/references/guides/diagrams/sequence-diagrams.md`
- Spring Boot, FastAPI, React, ETL, Node, Java code → `$CLAUDE_SKILL_DIR/references/guides/code-to-diagram/README.md` + matching `$CLAUDE_SKILL_DIR/examples/<framework>/README.md`
- design document, full docs → `$CLAUDE_SKILL_DIR/assets/*-design-template.md` + relevant diagram guide
- unicode, symbols, emoji → `$CLAUDE_SKILL_DIR/references/guides/unicode-symbols/guide.md`
- extract diagrams, validate mermaid → `$CLAUDE_SKILL_DIR/scripts/extract_mermaid.py`
- convert to image, PNG, SVG → `$CLAUDE_SKILL_DIR/scripts/mermaid_to_image.py`
- syntax error, troubleshoot → `$CLAUDE_SKILL_DIR/references/guides/troubleshooting.md`
- **any diagram generation** → always run `$CLAUDE_SKILL_DIR/scripts/resilient_diagram.py` to produce `.mmd` + `.png`

## Resilient Workflow

**CRITICAL:** Recommended approach for ALL diagram generation.

**Full Guide:** `$CLAUDE_SKILL_DIR/references/guides/resilient-workflow.md`

**Key Principles:**
- ALWAYS generate both a `.mmd` source file AND an image file (`.png`) — never just the markdown
- NEVER add a diagram to markdown until it passes validation

**Error Recovery Priority:**
1. `$CLAUDE_SKILL_DIR/references/guides/troubleshooting.md` (28 documented errors)
2. `perplexity_ask` MCP for syntax questions
3. `brave_web_search` MCP for recent solutions
4. `WebSearch` tool as fallback

## High-Contrast Styling

**ALL diagrams MUST use high-contrast colors with explicit `color:` in every `classDef`:**

```
classDef primary fill:#90EE90,stroke:#333,stroke-width:2px,color:darkgreen
classDef secondary fill:#87CEEB,stroke:#333,stroke-width:2px,color:darkblue
classDef database fill:#E6E6FA,stroke:#333,stroke-width:2px,color:darkblue
classDef error fill:#FFB6C1,stroke:#DC143C,stroke-width:2px,color:black
```

**Rules:** Light background → Dark text. Dark background → Light text.

## Intake

Before doing anything else, always confirm the diagram type with the user:

> "What type of diagram would you like to create? For example: swimlane/process flow, sequence, architecture, deployment, or code-to-diagram."

If the type is already clear from the request, confirm it instead of asking openly:

> "It sounds like you want a [type] diagram — is that right?"

Wait for confirmation, then proceed to routing.

## Workflow Summary

1. **Intake** → Ask what type of diagram the user wants if not already clear
2. **Analyze user intent** → Determine diagram type, document type, or action needed
2. **Load appropriate guide(s)** → Read only what's needed (token efficient)
3. **Apply templates and patterns** → Use examples from guides
4. **Generate diagram code** → Write Mermaid code following guide patterns
5. **ALWAYS run resilient workflow** → Save `.mmd` + generate `.png` via `resilient_diagram.py`, then embed image reference in markdown — NEVER skip this step, NEVER save diagram code only as `.md`


## Best Practices

1. **Single Responsibility**: One diagram = One concept
2. **Unicode Enhancement**: Always use semantic symbols for clarity
3. **High Contrast**: Never skip the `color:` property in styles
4. **Validate Early**: Use scripts to catch syntax errors
5. **Load On-Demand**: Only read guides needed for the specific request
