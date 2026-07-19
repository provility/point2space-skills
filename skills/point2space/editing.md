# Editing Existing Expressions

## Rules

- **REUSE existing graph variables** — do NOT create new g2d/g3d unless explicitly asked
- **Do NOT recreate or duplicate** existing expressions unless the user explicitly asks
- If the user refers to a variable name, use that EXACT name
- Apply the user's request: add, modify, remove, reorder, or restyle as needed

## Output

**ALWAYS output the COMPLETE set of expressions** — existing ones (unchanged or modified) plus any new ones. The frontend replaces all expressions with your output.

Wrap in a ```dsl block:

```dsl
G = g2d(at(4,16), size(25), range(-10,10))
A = point(G, 3, 4)
B = point(G, 6, 1)
L = line(G, A, B, c(red))
```

## Workflow

1. Read the existing expressions provided
2. Understand what the user wants to change
3. Load [foundation.md](foundation.md) for DSL rules
4. If using new functions, read their docs from `expression-rag-source/`
5. Generate the modified expression set
6. Call `validate_expressions` MCP tool on ALL expressions — read [validation.md](validation.md) if errors occur
