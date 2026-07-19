# Expression RAG Source

Reference documentation for Point2Space Studio expressions. Each JSON file documents one expression's syntax, arguments, variants, and examples.

## Rules for Writing Documentation

### Pattern Matching Uses ASCII Math, Never LaTeX

Users write ASCII math (e.g., `"x^2 + y^2"`), never raw LaTeX (e.g., `"\\frac{d}{dx}"`). The system converts ASCII math to LaTeX internally — this is invisible to the user. All examples, patterns, and string arguments in the documentation must use ASCII math.

### No Internal Implementation Details

Documentation is for end users. Never expose internal implementation details such as:
- Internal wrapping mechanisms
- How rendering engines (MathJax, KaTeX) process content
- Internal AST structures or node names
- Auto-generated variable suffixes or labels

### Point Tuples and Natural Syntax

The system supports native point tuples `(x, y)`, vector literals `{x, y}`, and matrix literals `[[a, b], [c, d]]`. These can be used directly in assignments and expressions:

```
A = (2, 1)
B = 1.3 * (4, 2)
v = {2, 3}
M = [[cos(pi/4), -sin(pi/4)], [sin(pi/4), cos(pi/4)]]
```

Line, vector, and point expressions accept tuples directly:
```
L = line(G, (2, 1), (4, -2))
L = line(G, A, B)
V = vector(G, (0,0), (0,0) + v(pi/2))
P = point(G, (3, 4))
P = point(G, M * (4, 0))
```

Functions, variables, and vectors use natural definition syntax (no `definition()` wrapper):
```
f(x) = x^2 + 2x + 1
R = 4
a = {2, 3}
```

### Dedicated Point Expressions

Point coordinate modes use dedicated expressions, NOT `type()` variants of `point()`:
- `polarpoint(G, radius, angle)` — polar coordinates
- `ratiopoint(G, shape, ratio)` — ratio along a shape
- `anglepoint(G, shape, angle)` — angle on circle/ellipse
- `plotpoint(G, plot, x)` — point on plot at x-value

### Dedicated Expressions (No type() Variants)

These are standalone expressions — do NOT document them as `type()` variants of another expression:
- `write()` — pen-traced math text (MathJax). Now unified: handles exclude(), type(invert), sub() internally (replaces old writeonly/writewithout/writesub)
- `print()` — instant math text (KaTeX)
- `text()` — plain text (alias for title())
- `title()` — plain text with animation effects
- `exclude()` — term filter used within write() for selective animation
- `select()` / `selectexcept()` — term selection from math text

### Conics Use point() for Centers

Ellipse, parabola, and hyperbola require `point()` or variable for center/vertex — not raw x, y numbers. Rotation uses `rotation(angle)` expression, not raw numbers.

### Styling Uses Expression Wrappers

Colors, fills, and opacity use expression wrappers:
```
c(red)          # stroke/text color
fc(blue)        # fill color
fo(0.3)         # fill opacity
fs(24)          # font size (was f(), now fs())
s(2)            # stroke width
so(0.5)         # stroke opacity
t(2)            # animation duration
dash(5, 3)      # dashed stroke
```

Do not document raw color strings or numbers as style arguments.

## JSON Structure

```json
{
  "expression": "name",
  "description": "What it does and when to use it",
  "utility": "One-line summary",
  "arguments": [
    {
      "name": "argName",
      "type": "expectedType",
      "required": true,
      "description": "What this argument does"
    }
  ],
  "alternates": [
    {
      "variant": "Variant name",
      "syntax": "expression(args)",
      "description": "When to use this variant"
    }
  ],
  "examples": [
    {
      "scenario": "What this example demonstrates",
      "expression": "actual_expression_string"
    }
  ]
}
```

## Directory Structure

| Directory | Contents |
|-----------|----------|
| `geometry/` | point, polarpoint, ratiopoint, anglepoint, plotpoint, angledef, line, circle, arc, polyline, complex, extendline, hline, vline, perp, pll, tangent-line |
| `vectors/` | vector, vecsum, vecdiff, vecproject, mag, dot, cross, polarvector, componentx, componenty, decomposeparallel, decomposeperp |
| `triangle-polygon/` | polygon, rect, square, sss, sas, asa, aas, angle, item, property, hatch |
| `conics/` | ellipse, parabola, hyperbola, conic-properties |
| `plotting/` | plot, parametric, polar, implicit, tangent, area-under, area-between, asymptote, paraplot, implicitplot, inequalityplot, piecewiseplot, polarplot |
| `mathtext/` | write, print, text, title, paragraph, exclude, select, comment, textcopy, textmove, textreplace, textreveal, textswap, textscale, textsplit, textgather |
| `graph2d/` | g2d (type mandatory: geometry/plotting/trig/invtrig/log/exp/complex), grid, numberline, pan, zoom, polar, range |
| `graph3d/` | g3d, axes3d, grid3d, range3d, space3d |
| `3d/` | point3d, line3d, vector3d, plane3d, curve3d, sphere3d, polygon3d, sector3d, dashedline3d, plot3d, angle3d, measure3d, distance3d, label3d, rightangle3d, pll3d, perp3d, intersect3d, placeat3d, trace3d, decompose3d, project3d, reflect3d, section3d, dircos3d, collinear3d, coplanar3d, bisector3d, area3d, tetrahedron3d, skew3d, lineargrid3d, linearcomb3d, pointcloud3d, normalplane3d, pointsplane3d, spanplane3d, interceptplane3d, paraplane3d, parametricline3d, symmetricline3d, cartesianline3d, pointvectorline3d, directionline3d |
| `3d-vectors/` | vector3d, vecsum3d, vecdiff3d, vecproject3d, norm3d, cross3d, boxprod, vectorscale3d, vectortrace3d |
| `3d-transforms/` | translate3d, rotate3d, scale3d, parallelshift3d, forward3d, backward3d, reverse3d, chain3d |
| `3d-plotting/` | plot3d, curve3d, tangent-plane, paraplot3d, implicit3d, piecewiseplot3d |
| `solids/` | cube3d, box3d, prism3d, cylinder3d, cone3d, doublecone3d, frustum3d, spheresolid3d, hemisphere3d, pyramid3d, tetrahedronsolid3d, octahedron3d, fill3d, drain3d, slice3d, unfold3d, extractsolid3d, combine3d, revolvedisk, revolvewasher, revolveshell, extrude3d, fold |
| `wireframe/` | sheetrect, sheetcircle, cut, cuttype, roll, cubenet, prismnet, pyramidnet, netfold, refold, tetrahedronnet, octahedronnet, sweep, taper, slice |
| `transforms/` | rotate, translate, scale, reflect, reverse, forward, backward, chain, placeat, rotateclone, translateclone, scaleclone |
| `analytical/` | intersect, perpendicular, project, reflect, parallelshift |
| `styling/` | color, highlight, highlightfill, highlightfill3d, fill-image, fill-color, fill-opacity, stroke-width, stroke-opacity, font-size (fs), font-family (ff), time (t), dash, nodraw, defer, precision (p), padding (pd), line-height (lh), border, border-radius (br), shadow, layout, hide, show, showonly, showall, fadein, fadeout, focus, outline, pattern, nolabel |
| `positioning/` | at, at-expression, buff, placeat, place, size |
| `algebra/` | solve, integral, limit, expand, simplify, det, transpose, inverse, mtrace, matrix, matrixrow, matrixdef, matrixaccess, polydiv |
| `function/` | funcdef (natural function definition syntax) |
| `assignment/` | assignment |
| `animator/` | animator, media, notes |
| `animators/` | variableanimator, pointanimator, lineanimator, vectoranimator, polygonanimator, playanimator, effect, riemann, piecewise, region, rejoin, point3danimator, line3danimator, vector3danimator |
| `annotations/` | annotationline, annotationrect, annotationellipse, annotationcurve, annotationpoly, annotationstep, tickmark, questionmark, crossmark, stepsub, stepcombine, stepimplies, callout, distancemarker, hatch, overbrace, underbrace |
| `correlate/` | arrow |
| `label/` | label, label-at-point, label-at-line, label-at-angle, label-at-expression, label-patterns |
| `table/` | table, header, tracetable, field, slidingwindow, table-patterns |
| `numberline/` | nl, nlpoint, ray, interval, ineq, absineq, nlblock, nlspan, nlunion, nlintersect, nldiff, nllabel |
| `interactive/` | dropdown, inputstepper, inputnumber, inputmath, inputaiaction, display, setvalue, computedproperty |
| `boolean/` | and, or, diff, xor |
| `sets/` | venn, complement, ptree, branch, ptreetrace, ptreeexclude |
| `probability/` | normdist, normcdf, binomial, pmf, dcdf, bprob, sspace, event, given, dice, coin, cards, outcomes, bags, balls, pick, frombag, permutation, combination, set, choose, btree, trial, repeat, some, sum, match, filter, suit, face, rank, color, histogram |
| `arithmetic/` | hcfbar, lcmbar, ladderlcm |
| `p5/` | p5sheet, p5-api-vectors (pvector, distanceMarker, curveMarker, angleMarker), p5-api-utilities (canvas, fields, phaseStepper, bridge) |
| `charts/` | barchart, linechart, scatter, piechart, cdfchart, prob |
| `statistics/` | stattable, values, statcolumn |
| `playback/` | play, pause, clear, playaction, playinputaction, playinputanimator, playnavigator |
| `navigation/` | scrollto, goto, section, summary, fragment |
| `data/` | datatable, plottable, data |
| `special/` | view, extrusionprofile, axis, crossplane, noplanes, part, spline, parallelmarker, vx |
| `visualization/` | tracepoint, hidehighlight, inner, outer, plotlines, ref, rollout, visibility, xcol, xval, ycol, yval, zoomout, desc, vectortrace |
| `utility/` | tip, box, resize, config, mref, interpolate |
| `internal/` | internal-expressions (reference for non-user-facing helpers) |
| `image/` | image |
