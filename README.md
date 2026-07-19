# point2space-skills

Claude Code skills for [Point2Space](https://point2space.com) — an interactive math visualization and lesson builder.

## What's Included

The `point2space` skill enables Claude Code to generate, edit, and validate Point2Space DSL expressions for creating interactive math visualizations and multi-slide lessons.

**Capabilities:**
- Generate DSL code for 2D/3D math visualizations (geometry, plotting, vectors, charts, animations)
- Create multi-slide interactive math lessons with pedagogical sequencing
- Edit and modify existing expression sets
- Validate expressions against the DSL grammar with automated error correction

## Installation

```bash
npx skills add point2space-skills
```

Or clone directly:

```bash
git clone https://github.com/provility/point2space-skills.git
# Copy skills/point2space/ into your project's .claude/skills/
```

## Prerequisites

Connect to the Point2Space MCP server for expression validation, generation, and lesson management. The skills provide the DSL knowledge; the MCP provides the tools.

## Skill Structure

```
skills/point2space/
├── SKILL.md                    # Skill manifest and entry point
├── foundation.md               # DSL grammar, rules, and constraints
├── editing.md                  # Guide for modifying existing expressions
├── lesson-creation.md          # Multi-slide lesson creation workflow
├── validation.md               # QA checklist and error correction loop
├── expression-rag-source/      # 537 JSON docs — complete DSL function reference
└── reference/samples/          # 13 real lesson examples
```

## DSL Overview

Point2Space DSL creates interactive math explanations combining:
- **Graphing** — 2D and 3D coordinate systems with configurable ranges
- **Geometry** — points, lines, circles, polygons, angles, conics
- **Plotting** — functions, parametric curves, implicit equations, polar plots
- **3D** — solids, surfaces, vectors, transforms
- **Animation** — variable animators, point animators, playback control
- **Text** — math notation (AsciiMath), labels, annotations, titles
- **Data** — charts, tables, statistics, number lines

Example:
```dsl
G = g2d(at(4,16), size(25), range(-10,10))
f(x) = x^2 - 4
p1 = plot(G, "f(x)", c(blue))
A = point(G, -2, 0, c(red))
B = point(G, 2, 0, c(red))
```

## License

MIT
