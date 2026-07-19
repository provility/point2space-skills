# Validation & QA

Assume there are problems. Your job is to find them.

## Expression Validation

### Validate

Use the MCP `validate_expressions` tool to validate every expression you generate:

```
validate_expressions({ expressions: ["expr1", "expr2", ...] })
```

Response format:
```json
{
  "results": [
    {"expression": "...", "status": "valid", "detail": null},
    {"expression": "...", "status": "error", "detail": "Unknown function or command: \"foo\"", "type": "unknown-function", "keyword": "foo"}
  ]
}
```

**This step is mandatory.** Never present expressions to the user without validating them first.

### Classify and Fix Errors

For each failed expression, classify by error type:

| Error Type | Action |
|---|---|
| Unknown function | The `keyword` field tells you what failed. Search `expression-rag-source/` for the correct function name: `find expression-rag-source -name "*.json" \| grep -i "<keyword>"`. Read the doc. Fix. |
| Wrong arguments | Re-read the JSON doc for that function — check `arguments` and `alternates` for correct signatures. |
| Syntax error | DSL grammar violation — check against the grammar rules in foundation.md. Common causes: missing LHS variable, LaTeX instead of AsciiMath, bare function call without assignment. |
| Undefined variable | A variable is referenced before it's defined. Check expression ordering — dependencies must come first. |

### Correction Loop

1. Call `validate_expressions` with all expressions
2. For each error: read the relevant documentation from `expression-rag-source/`, understand WHY it failed, fix with correct syntax from the docs
3. Call `validate_expressions` again with the corrected expressions
4. Repeat up to **3 rounds**

**Don't retry blindly** — understand WHY each error occurred and fix using the docs.

### Final Decision

- All pass → proceed to QA checks below
- Still failing after 3 retries, majority valid → strip invalid expressions, return valid subset with a warning
- Majority invalid → report error, ask user to rephrase

---

## Text Best Practices

After validation passes, check all text expressions for correct wrapping:

**Math-mode expressions** (`write`, `desc`):
- Plain English words MUST be wrapped in `text()`
- Math stays unwrapped
- Example: `write(at(5,30), "text(Generating curve:) x = 2 + sin(y)")`

**Plain-text expressions** (`title`, `paragraph`, `comment`, `notes`):
- Math expressions MUST be wrapped in `math()`
- Plain text stays unwrapped
- Example: `title(at(5,5), "Understanding math(pi r^2)")`

If any text wrapping issues are found, fix them. Do NOT change anything else — no positions, colors, sizes, logic, or structure.

---

## Structural QA (Lessons)

When validating a multi-slide lesson, check all of the following:

### 1. Expression Dependency Order
For each slide, verify that every variable referenced is defined in a preceding expression within the same slide.
- `para_str_*` variables come from paraVariables — these are valid references
- `select()` can reference auto-generated names like `write_1_select_1`
- Deferred expressions with `defer()` can be referenced by later `play()` expressions

### 2. ParaVariable Consistency
- Every `para_str_*` referenced in expressions must exist in that slide's `paraVariables`
- Every entry in `paraVariables` should be referenced by at least one expression

### 3. Graph Argument Rule
- All 2D shape/plot functions must have a graph variable as first argument
- Check: `point()`, `line()`, `circle()`, `polygon()`, `plot()`, `label()`, `vector()`, etc.
- Exception: text expressions (`write`, `title`, `desc`, `paragraph`) do NOT take graph args

### 4. Slide Structure
- Each slide should have at least 2 expressions (minimum: graph + one shape)
- No slide should have more than 50 expressions
- `slideOrder` must list every slide ID exactly once
- `activeSlideId` must be a valid slide ID

### 5. Color Usage
- Only CSS named colors: `c(red)`, `fc(lightblue)`
- No hex codes: `c(#ff0000)` is invalid
- No RGB: `c(rgb(255,0,0))` is invalid

### 6. Position Sanity
- Graph containers should use reasonable sizes (10-30 range)
- Text expressions should be spaced at least 2-3 rows apart vertically

### 7. Naming Conventions
- No duplicate variable names within a slide
- Variable names should follow patterns: `write_1`, `point_1`, `graph_1`, `label_1`
- paraVariable names should follow: `para_str_1`, `para_str_2`

---

## Verification Loop

1. Generate expressions → `validate_expressions` → Fix errors → Re-validate
2. Run text best-practices check
3. Run structural QA checklist (for lessons)
4. Fix any issues found
5. Re-validate affected expressions

**Do not declare success until you've completed at least one fix-and-verify cycle.**
