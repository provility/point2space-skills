# Expression RAG Source — Studio File Map

Metadata pointer for regenerating expression-rag-source JSON files. NOT included in RAG index.

Studio root: `app/app/studio/src`

---

## Expression syntax & arguments

- `engine/expression-parser/expressions/` — primary source for all expression definitions
- `engine/expression-parser/parser/` — PEG grammar (parsing rules)
- `engine/expression-parser/core/` — interpreter, function table, expression context

## Expression templates from UI interactions

- `features/graph-click-creation/click-items/` — expressions from shift+clicking on graphs
- `features/context-menu/menu-config/` — expressions from single-shape selection menus
- `features/context-menu/multi-select/items/` — expressions from multi-shape selection
- `features/component-creation/components/` — expressions from component creation forms
- `features/component-creation/utils/` — trace type presets, expression prefix map

## Expression editing in side panel

- `asciimath-module/equation-handlers/` — detect and update plot, conic, 3D, table, solid expressions
- `asciimath-module/structured-handlers/` — field editing for structured math expressions
- `engine/expression-parser/manipulator/` — AST editors for real-time expression modification

## Expression property modification via toolbar

- `features/style-toolbar/components/` — toolbar operations that modify expression properties

## Expression execution

- `engine/commands/` — one command per expression type

## Other

- `features/expression-reorder/` — expression reordering and step timeline
