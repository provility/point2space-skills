# Multi-Slide Lesson Creation

## Input

You receive a folder path. The folder contains at least one of:
- `description.txt` or `description.md` — topic and goals
- A `.json` file — sample-prompt format with a `prompt` field describing the lesson

Optional extras:
- `steps.json` — mathematical step-by-step breakdown
- An image file (png/jpg) — visual reference of expected output

Read whatever exists in the folder to understand what lesson to create.

## Workflow

### 1. Understand the Task

1. **Read the input folder** — `ls` the folder, then read the description/prompt and any steps or images.
2. **Study a relevant reference sample** — scan `reference/samples/` for a lesson similar to your topic. Read 1-2 relevant samples fully to understand real lesson structure and expression patterns. These are ground truth.
3. **Load the DSL foundation** — read `foundation.md` for complete grammar, styling, text rules, and constraints.

### 2. Research Expressions

Before writing any code, find and read the documentation for every DSL function you plan to use.

1. List what functions the lesson needs (e.g., g2d, circle, point, label)
2. Find their docs: `find expression-rag-source -name "*.json" | grep -i "<function_name>"`
3. Read each JSON file — pay attention to `arguments`, `alternates` (syntax variants), and `examples`
4. Check the `_requires` field — if a function requires others, read those docs too

**NEVER guess DSL syntax. Always read the docs first.**

### 3. Plan the Lesson

Before generating ANY expressions:

1. **Understand the mathematics** — identify the theorem, formula, or concept being illustrated
2. **Compute all values** — side lengths, angles, areas, intercepts, coordinates of every point
3. **Design the slide sequence** — what concept does each slide teach, in what order
4. **Plan the complete construction per slide** — list every geometric element needed
5. **Calculate exact coordinates** — derive positions from the math, never guess

### 4. Generate

Create DSL expressions for each slide:
- **One concept per slide**, 3-7 slides per lesson
- **Dependencies first** — graph before shapes, shapes before labels
- **Pedagogical order** — the way a teacher would explain on a whiteboard
- Use `t()` delays to pace animation (0.5-2s between major steps)

### 5. Validate

Read [validation.md](validation.md) for the full error correction loop and QA checklist.

If the `validate_expressions` MCP tool is available, call it on ALL expressions:
```
validate_expressions({ expressions: ["expr1", "expr2", ...] })
```

If MCP is not available, self-validate each expression against the DSL grammar in [foundation.md](foundation.md) and the expression docs in `expression-rag-source/`.

**Do not declare success until validation passes. Fix errors and re-validate.**

### 6. Write lesson.json

Once all expressions validate and QA passes, assemble the lesson JSON and write it to the input folder.

## Output Format

```json
{
  "assets": [],
  "slides": {
    "<slide_id>": {
      "id": "<slide_id>",
      "name": "Slide Name",
      "expressions": ["expr1=...", "expr2=...", "", "", ""],
      "paraVariables": [{"name": "para_str_1", "value": "..."}]
    }
  },
  "settings": {},
  "slideOrder": ["<slide_id_1>", "<slide_id_2>"],
  "activeSlideId": "<first_slide_id>",
  "id": "<uuid-v4>",
  "name": "Lesson Title"
}
```

**Slide IDs:** `slide_<timestamp>_<random9chars>`
**Lesson ID:** UUID v4
**Expressions array:** End with empty strings for editor padding.
**paraVariables:** Include any `para_str_*` variables referenced in expressions.
