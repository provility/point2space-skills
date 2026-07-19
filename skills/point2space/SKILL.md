---
name: point2space
description: "Use this skill any time Point2Space DSL expressions or lessons are involved. This includes: generating DSL code for math visualizations; creating multi-slide interactive math lessons; editing or modifying existing expressions; validating or fixing DSL expressions. Trigger whenever the user mentions plotting, drawing shapes, creating a lesson, math visualization, DSL expressions, Point2Space, or references geometric/mathematical constructions, graphs, charts, animations, or interactive math content."
metadata:
  author: Sharfudeen Ashraf
  version: "1.0.0"
  homepage: https://point2space.com
  source: https://github.com/provility/point2space-skills
---

# Point2Space Skill

## Quick Reference

| Task | Guide |
|------|-------|
| Create a single visualization | Read [foundation.md](foundation.md), then generate |
| Create a multi-slide lesson | Read [lesson-creation.md](lesson-creation.md) |
| Edit existing expressions | Read [editing.md](editing.md) |
| Validate/fix expressions | Read [validation.md](validation.md) |

Before generating ANY code, read [foundation.md](foundation.md) for the complete DSL grammar, rules, and constraints.

---

## Expression Documentation

`expression-rag-source/` (co-located with this skill) is the **single source of truth** for DSL syntax. Each expression has a JSON file with arguments, alternates (syntax variants), examples, and `_requires` dependencies.

**NEVER guess DSL syntax. Always read the JSON docs first.**

### How to Find Documentation

All paths are relative to this skill's directory (`skills/point2space/`).

Users won't always use the exact function name. Use these search strategies in order:

```bash
# 1. Direct name match (try first)
find skills/point2space/expression-rag-source -name "*circle*" -name "*.json"

# 2. Broader partial/fuzzy match (user says "perpendicular", function is "perp")
find skills/point2space/expression-rag-source -name "*.json" | grep -i "perp"

# 3. Search inside JSON files for description/utility text (user says "tangent to curve")
grep -rl "tangent" skills/point2space/expression-rag-source --include="*.json"

# 4. Browse a likely category when keyword search fails
ls skills/point2space/expression-rag-source/geometry/
ls skills/point2space/expression-rag-source/plotting/

# 5. Search by concept — check multiple related categories
find skills/point2space/expression-rag-source -name "*.json" | xargs grep -li "angle"
```

**Search tips:**
- User says "dot product" → search `dot`, check `vectors/` and `3d-vectors/`
- User says "shade the area" → search `area`, `fill`, check `plotting/`
- User says "dashed line" → the style is `dash()` in `styling/`, the shape is `line` in `geometry/`
- User says "animate" → check both `animators/` and `animator/` directories
- When in doubt, use the Directory Map below to pick the right category, then `ls` it

### Directory Map

| Category | Path | Contains |
|----------|------|----------|
| **Graph Containers** | | |
| 2D graph | `expression-rag-source/graph2d/` | g2d, g2d-trig, grid, numberline, pan, zoom |
| 3D graph | `expression-rag-source/graph3d/` | g3d, axes3d, grid3d, range3d |
| **2D Geometry** | | |
| Points, lines, circles, arcs | `expression-rag-source/geometry/` | point, line, circle, arc, arcmarker, polyline, hline, vline, norm, perp, tangent-line, and point transforms (reflect, rotate, translate, scale, project, intersect, midpoint, vertex, etc.) |
| Triangles, polygons, angles | `expression-rag-source/triangle-polygon/` | angle, hatch, aas, asa, sss, sas, polygon constructions, intersect |
| Conics | `expression-rag-source/conics/` | ellipse, parabola, hyperbola, conic-properties |
| Analytical (intersect, perp, parallel) | `expression-rag-source/analytical/` | intersect, perpendicular, parallelshift, project, reflect, rotate |
| **Plotting** | | |
| 2D plots | `expression-rag-source/plotting/` | area-under, area-between, asymptote, implicit-plot, inequalityplot, parametric, piecewise, plot, polar, riemann, tangent |
| 3D plots | `expression-rag-source/3d-plotting/` | plot3d, paraplot3d, curve3d, implicit3d, surface, piecewiseplot3d |
| **3D** | | |
| 3D shapes | `expression-rag-source/3d/` | sphere, cone, cylinder, cube, prism, pyramid, torus, plane3d, line3d, point3d, arc3d, angle3d, bisector3d, etc. |
| 3D vectors | `expression-rag-source/3d-vectors/` | vector3d, cross3d, dot3d, norm3d, vecdiff3d, vecsum3d |
| 3D transforms | `expression-rag-source/3d-transforms/` | rotate3d, translate3d, scale3d, reflect3d, forward3d, backward3d, chain3d |
| **Vectors** | | |
| 2D vectors | `expression-rag-source/vectors/` | vector, cross, dot, norm, polarvector, vecdiff, vecsum, unitvector, projection |
| **Text & Labels** | | |
| Math text (write, desc) | `expression-rag-source/mathtext/` | comment, paragraph, select, selectcell, exclude |
| Labels | `expression-rag-source/label/` | label, label-at-angle, label-at-expression, label-at-line, label-at-point, etc. |
| Annotations | `expression-rag-source/annotations/` | annotationcurve, annotationellipse, annotationline, annotationpoly, brace, callout, overbrace, underbrace |
| Visualization (write, desc, title) | `expression-rag-source/visualization/` | write, desc, title, col, gutter, inner, row |
| Text techniques | `expression-rag-source/text-techniques/` | arrow-connections, diagram-to-equation, layered-reveal, correlation-pedagogy |
| **Styling & Positioning** | | |
| Styling | `expression-rag-source/styling/` | color, fill, opacity, stroke, dash, arrowhead, border, shadow, arcsize, highlight, hide, show, fadein |
| Positioning | `expression-rag-source/positioning/` | at, anchor, buff, placement |
| **Animation & Playback** | | |
| Animators | `expression-rag-source/animators/` | variableanimator, pointanimator, lineanimator, line3danimator, effect, piecewise |
| Animator (media, notes) | `expression-rag-source/animator/` | animator, media, notes |
| Playback | `expression-rag-source/playback/` | play, pause, clear |
| Transforms | `expression-rag-source/transforms/` | translate, rotate, scale, reflect, forward, backward, chain, placeat |
| Traces | `expression-rag-source/traces/` | trace, trace-identity, trace-trig |
| **Data & Charts** | | |
| Charts | `expression-rag-source/charts/` | barchart, linechart, piechart, bellcurve, ndist, cdfchart, scatterplot |
| Tables | `expression-rag-source/table/` | table, header, table-patterns |
| Data | `expression-rag-source/data/` | data, datatable, plottable |
| Statistics | `expression-rag-source/statistics/` | stattable, statcolumn, values |
| Number lines | `expression-rag-source/numberline/` | nl, nlpoint, ineq, absineq, interval |
| **Solids & Wireframes** | | |
| 3D solids | `expression-rag-source/solids/` | solid shapes, operations (fill3d, unfold3d, slice3d, combine3d), measurements (solidmeasure, solidangle, solidface, solidarea), positioning (loc3d, lockat) |
| Wireframes & nets | `expression-rag-source/wireframe/` | area, conefold, cubenet, basketfold, etc. |
| **Other** | | |
| Algebra | `expression-rag-source/algebra/` | solve, expand, integral, limit, det, inverse, transpose, simplify |
| Assignment | `expression-rag-source/assignment/` | assignment syntax |
| Function | `expression-rag-source/function/` | funcdef |
| Boolean | `expression-rag-source/boolean/` | and, or, diff, xor |
| Sets | `expression-rag-source/sets/` | venn, ptree, complement |
| Interactive | `expression-rag-source/interactive/` | dropdown, computedproperty, setdropdownvalue |
| Navigation | `expression-rag-source/navigation/` | section, goto, scrollto, fragment, summary |
| Image | `expression-rag-source/image/` | image |
| Correlate | `expression-rag-source/correlate/` | arrow, correlate |
| Special | `expression-rag-source/special/` | axis, crossplane, dynamiccall, extrusionprofile |
| Utility | `expression-rag-source/utility/` | box, config, interpolate, map, mathfunction |

### Reading Expression Docs

Each JSON doc contains:
- `arguments` — required and optional parameters
- `alternates` — different syntax variants for the same function
- `examples` — real usage with scenario descriptions
- `_requires` — dependencies (e.g., circle requires g2d and point)

When planning code, read the docs for **every function you plan to use** plus their `_requires` dependencies.

---

## Validation

Validate all generated expressions before declaring success using the MCP `validate_expressions` tool:

```
validate_expressions({ expressions: ["expr1", "expr2"] })
```

Read [validation.md](validation.md) for the full error correction loop and QA checklist.

---

## MCP Tools Available

This skill is designed to work with the Point2Space MCP server. Use these tools:

| Tool | Purpose |
|------|---------|
| `validate_expressions` | Validate DSL expressions against the grammar |
| `generate_expressions` | Generate expressions from natural language |
| `search_expression_docs` | Search expression documentation |
| `search_samples` | Search sample prompts for inspiration |
| `list_sample_categories` | List available sample categories |
| `create_lesson` | Create a new lesson with slides |
| `update_lesson` | Update lesson metadata |
| `get_lesson` | Retrieve a lesson by ID |
| `list_my_lessons` | List user's lessons |
| `add_slide` | Add a slide to an existing lesson |
| `update_slide_expressions` | Update expressions on a slide |
