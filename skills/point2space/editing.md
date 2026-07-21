# Editing Existing Expressions

## Rules

- **REUSE existing graph variables** — do NOT create new g2d/g3d unless explicitly asked
- **Do NOT recreate or duplicate** existing expressions unless the user explicitly asks
- If the user refers to a variable name, use that EXACT name
- Apply the user's request: add, modify, remove, reorder, or restyle as needed

## Output

**ALWAYS output the COMPLETE set of expressions** — existing ones (unchanged or modified) plus any new ones. The frontend replaces all expressions with your output.

### Show a Change Diff First

Display a ```diff block so the user can see what changed at a glance:

```diff
  G = g2d(at(4,16), size(25), range(-10,10))
  A = point(G, 3, 4)
- B = point(G, 6, 1)
+ B = point(G, 7, 2)
  L = line(G, A, B, c(red))
+ C = point(G, 0, 3, c(blue))
```

- `+` = new or replacement line (shown in green)
- `-` = removed or old version of a modified line (shown in red)
- ` ` (space) = unchanged (neutral)
- For **modified** expressions: show old with `-` then new with `+` on adjacent lines
- Include ALL expressions (unchanged too) for full context

### Then Show the Final Clean Block

After the diff, show the resulting expressions in a ```dsl block — this is what the frontend parses:

```dsl
G = g2d(at(4,16), size(25), range(-10,10))
A = point(G, 3, 4)
B = point(G, 7, 2)
L = line(G, A, B, c(red))
C = point(G, 0, 3, c(blue))
```

## Workflow

1. Read the existing expressions provided
2. Understand what the user wants to change
3. Load [foundation.md](foundation.md) for DSL rules
4. If using new functions, read their docs from `expression-rag-source/`
5. Generate the modified expression set
6. Validate ALL expressions — call `validate_expressions` MCP tool if available, otherwise self-check against the DSL grammar and expression docs. Read [validation.md](validation.md) if errors occur
