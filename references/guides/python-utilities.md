# Python Utilities

Three standalone Python scripts (stdlib only, Python 3.7+). Requires `@mermaid-js/mermaid-cli` (`mmdc`) for validation and rendering.

```bash
npm install -g @mermaid-js/mermaid-cli
```

## extract_mermaid.py — Extract & Validate

```bash
# List all diagrams in a file
python scripts/extract_mermaid.py document.md --list-only

# Extract to separate .mmd files
python scripts/extract_mermaid.py document.md --output-dir diagrams/

# Validate all diagrams
python scripts/extract_mermaid.py document.md --validate

# Replace with image references (for Confluence upload)
python scripts/extract_mermaid.py document.md --replace-with-images \
  --image-format png --output-markdown output.md
```

## mermaid_to_image.py — Convert to PNG/SVG

```bash
# Single conversion
python scripts/mermaid_to_image.py diagram.mmd output.png

# With custom settings
python scripts/mermaid_to_image.py diagram.mmd output.svg \
  --theme dark --background white --width 1200

# Batch convert directory
python scripts/mermaid_to_image.py diagrams/ output/ --format png --recursive

# From stdin
echo "graph TD; A-->B" | python scripts/mermaid_to_image.py - output.png
```

## resilient_diagram.py — Full Workflow with Error Recovery

```bash
python scripts/resilient_diagram.py \
    --code "flowchart TD; A-->B" \
    --markdown-file design_doc \
    --diagram-num 1 \
    --title "process_flow" \
    --format png \
    --json
```

**Output:** Both `.mmd` and `.png` in `./diagrams/` using naming convention:
```
./diagrams/<markdown_file>_<num>_<type>_<title>.mmd
./diagrams/<markdown_file>_<num>_<type>_<title>.png
```
