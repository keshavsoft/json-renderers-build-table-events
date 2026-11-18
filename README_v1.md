# json-renderers-build-table-events

> **Instruction-Driven Event Orchestration and Declarative DOM Traversal for Table Components.**

`json-renderers-build-table-events` provides a clean, decoupled interactivity layer for table components. It separates pure HTML rendering from component event handling, eliminating hardcoded DOM selectors and fragile ID dependencies.

---

## The Core Philosophy: Rendering vs. Events

In modern component architecture, conflating table layout rendering with event wiring creates tight coupling and DOM focus destruction (e.g. repainting an entire component on every keystroke wipes the search bar and kills typing focus).

This library establishes a clean boundary:
1. **`json-renderers-build-table`**: Pure structural rendering (`render()`).
2. **`json-renderers-build-table-events`**: Behavior, event delegation, and declarative traversal (`hookTableEvents()`).

```
                    ┌───────────────────────────────┐
                    │  json-renderers-build-table   │
                    │      (Pure Rendering)         │
                    └──────────────┬────────────────┘
                                   │
                                   ▼
┌────────────────────────────────────────────────────────────────────────┐
│                   json-renderers-build-table-events                    │
├────────────────────────────────┬───────────────────────────────────────┤
│    Declarative Delegation      │      Instruction-Driven Traversal     │
│       (bindings.json)          │          (instructions.json)          │
├────────────────────────────────┼───────────────────────────────────────┤
│ • input  -> onSearch           │ • closest: 'tbody tr'                 │
│ • click  -> onRowClick         │ • querySelectorAll: 'td'              │
│ • click  -> onRowAction        │ • mapNamedValues / readValue          │
│ • click  -> onFooterSave       │ • powered by dom-primitive-engine     │
└────────────────────────────────┴───────────────────────────────────────┘
```

---

## Key Features

- **No Premature Abstractions**: Every action (`onSearch`, `onRowClick`, `onRowAction`, `onFooterSave`) is a self-contained feature folder with its own `index.js` and `instructions.json`.
- **Zero Hardcoded DOM Selectors in JS**: All element lookups, parent traversals (`closest`), and control queries are declared in serializable JSON recipes executed via `dom-primitive-engine`.
- **Persistent Input Focus**: During live search, only the inner table rows are repainted—the search bar and feedback displays are never destroyed.
- **Delegated at Container Level**: A single container listener handles all row clicks, buttons, and search inputs with no memory leaks or listener bloat.

---

## Installation

```bash
npm install json-renderers-build-table-events
```

Or consume via CDN:
```html
<script src="https://keshavsoft.github.io/json-renderers-build-table-events/dist/v1/min.js"></script>
```

---

## Quick Start

```javascript
import { hookTableEvents } from "json-renderers-build-table-events";

// 1. Initial Render (using your renderer)
myTableRenderer({
    container: "dom-render-container",
    data: dataset,
    columns: columns
});

// 2. Hook Interactivity
hookTableEvents({
    container: "dom-render-container",
    dataset: dataset,
    columns: columns,
    renderTable: (filteredRecords) => {
        // Localized re-render of inner table
        myTableRenderer({ container: "table-content-container", data: filteredRecords });
    },
    onSearch: (query, filteredRecords) => {
        console.log(`Search: "${query}", ${filteredRecords.length} matches`);
    },
    onRowClick: (record, rowIndex) => {
        console.log(`Selected row #${rowIndex + 1}:`, record);
    },
    onRowAction: (action, record, rowIndex) => {
        console.log(`Action "${action}" on row #${rowIndex + 1}:`, record);
    },
    onSave: (newRecord, updatedDataset) => {
        console.log("New row saved:", newRecord);
    }
});
```

---

## Architecture Overview

```
src/
├── index.js                     <-- Root entrypoint
└── v1/
    ├── index.js                 <-- hookTableEvents orchestrator
    ├── state.js                 <-- Reactive state holder
    ├── registerGlobal/          <-- Global registration on window.ks.jsonRenderers.build.tableEvents
    ├── bindings/
    │   ├── bindings.json        <-- Declarative event routing
    │   └── index.js             <-- Delegated listener engine
    └── actions/                 <-- Self-contained feature folders
        ├── onSearch/
        │   ├── instructions.json
        │   └── index.js
        ├── onRowClick/
        │   ├── instructions.json
        │   └── index.js
        ├── onRowAction/
        │   ├── instructions.json
        │   └── index.js
        └── onFooterSave/
            ├── instructions.json
            └── index.js
```

### Example: Declarative Traversal (`onSearch/instructions.json`)

```json
[
  {
    "narration": "From search input, find parent container",
    "sourcePath": { "fileName": "root" },
    "func": "closest",
    "arguments": { "selector": "[data-role='table-parent'], .card-body, .container, div" }
  },
  {
    "narration": "Locate search feedback display element",
    "sourcePath": { "fileName": "previous" },
    "func": "querySelector",
    "arguments": { "selector": "#search-feedback, [data-role='search-feedback']" }
  }
]
```

---

## Global Browser Access

When loaded via a `<script>` tag, the engine automatically registers itself globally on:
```javascript
window.ks.jsonRenderers.build.tableEvents.hookTableEvents({ ... });
```

---

## License

MIT &copy; [KeshavSoft](https://github.com/keshavsoft)
