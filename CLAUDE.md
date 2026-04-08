# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Purpose

This is **design-doc-mermaid**, a Claude Code skill (v2.0) for generating Mermaid diagrams and design documents. It is not a compiled application — there is no build step. The skill is invoked by Claude Code via `SKILL.md`.

## No Build System

There are no `package.json`, `Makefile`, or test suites. The only runtime components are three standalone Python scripts (stdlib only, Python 3.7+) and the optional `@mermaid-js/mermaid-cli` (`mmdc`) for diagram validation and image rendering.

```bash
# Install optional CLI for validation/rendering
npm install -g @mermaid-js/mermaid-cli

# Extract diagrams from a markdown file
python scripts/extract_mermaid.py document.md --output-dir diagrams/

# Validate diagram syntax
python scripts/extract_mermaid.py document.md --validate

# Convert a .mmd file to PNG/SVG
python scripts/mermaid_to_image.py diagram.mmd output.png

# Full resilient workflow with error recovery
python scripts/resilient_diagram.py --code "[diagram_code]" --markdown-file design_doc --diagram-num 1
```

## Architecture: Hierarchical On-Demand Loading

The skill loads only what it needs (~2–5 KB) rather than the full reference set (~50 KB+). `SKILL.md` is the orchestrator — it contains a decision tree that maps user intent to a specific guide or template, which is then read and used to generate output.

```
User Request → SKILL.md decision tree → Load 1–2 relevant guides → Generate output
```

**Intent → Resource mapping:**

| User intent | Resource loaded |
|---|---|
| workflow, process, business logic | `references/guides/diagrams/activity-diagrams.md` |
| infrastructure, deployment, cloud | `references/guides/diagrams/deployment-diagrams.md` |
| system architecture, components | `references/guides/diagrams/architecture-diagrams.md` |
| API flow, service interactions | `references/guides/diagrams/sequence-diagrams.md` |
| code-to-diagram | `references/guides/code-to-diagram/` + matching `examples/<framework>/` |
| design document | `assets/*-design-template.md` + relevant diagram guide(s) |
| icons/symbols | `references/guides/unicode-symbols/guide.md` |
| extract/validate/convert | `scripts/extract_mermaid.py` or `scripts/mermaid_to_image.py` |

## Key Directories

- `SKILL.md` — skill definition and full decision tree (primary entry point)
- `references/guides/diagrams/` — four specialized diagram guides (activity, deployment, architecture, sequence)
- `references/guides/code-to-diagram/` — framework extraction patterns
- `references/troubleshooting.md` — 28 documented Mermaid syntax errors and fixes
- `assets/` — five design document templates (api, architecture, feature, database, system)
- `examples/` — framework-specific code samples (Spring Boot, FastAPI, React, Node, Java, Python ETL)
- `scripts/` — three Python utilities for extraction, rendering, and resilient generation

## Styling Convention

All diagrams use explicit `color:` properties in `classDef` and `style` declarations for WCAG 2.1 Level AA compliance (high-contrast). Always include foreground color alongside background color — never rely on defaults.

## Adding a New Diagram Type

1. Create a guide in `references/guides/diagrams/<type>-diagrams.md` following the structure of existing guides (templates, examples, best practices, troubleshooting).
2. Add the intent trigger and file path to the decision tree table in `SKILL.md`.
3. Update the guides table in the `## Available Guides and Resources` section of `SKILL.md`.
