# json-renderers-build-table-events

> Instruction-Driven Event Orchestration and Declarative DOM Traversal for Table Components.

[![npm version](https://img.shields.io/npm/v/json-renderers-build-table-events.svg)](https://www.npmjs.com/package/json-renderers-build-table-events)
[![Live Demo](https://img.shields.io/badge/Live-Demo-brightgreen.svg)](https://keshavsoft.github.io/json-renderers-build-table-events/)
[![npm downloads](https://img.shields.io/npm/dm/json-renderers-build-table-events.svg)](https://www.npmjs.com/package/json-renderers-build-table-events)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![CDN](https://img.shields.io/badge/CDN-dist%2Fmin.js-blue.svg)](https://keshavsoft.github.io/json-renderers-build-table-events/dist/v1/min.js)

---

## 📖 The Story: Why This Exists

### The Monolithic Problem
In early web and declarative architectures, table builders attempted to do everything in a single monolithic block: rendering HTML markup, binding click listeners, handling search filtering, reading form inputs, and manipulating the DOM.

This tight coupling created severe user experience and maintenance nightmares:
- **Typing Focus Destruction:** Whenever a user typed into a search filter, re-rendering the table often replaced the entire component DOM, stripping focus from the `<input>` element and abruptly halting user typing.
- **Brittle Selectors:** Logic was riddled with fragile, hardcoded element IDs (`#save-btn`, `event.target.parentElement.parentElement`), making UI refactoring painful and error-prone.
- **Listener Bloat & Leaks:** Attaching individual event listeners to every table row, button, or input field caused memory leaks and poor performance when datasets grew.

### The Breakthrough: Decoupling Rendering from Events
`json-renderers-build-table-events` was created to establish a clean, robust separation of concerns:
1. **Rendering is pure:** `json-renderers-build-table` (or any table renderer) is responsible solely for rendering table structures.
2. **Events are decoupled:** `json-renderers-build-table-events` manages all user interactions through event delegation and declarative JSON instruction recipes.

With this separation, live searches re-render **only** the inner table rows without touching the search bar, preserving keyboard focus seamlessly. Zero DOM selectors are hardcoded in JavaScript, and all actions run smoothly through a single delegated container listener.

---

## 🚀 Repo Usages & Documentation

### 1. Installation
Install the package via npm:
```bash
npm install json-renderers-build-table-events
```

Or consume it directly in the browser via CDN:
```html
<script src="https://keshavsoft.github.io/json-renderers-build-table-events/dist/v1/min.js"></script>
```

### 2. What is the Purpose?
The purpose of this repository is to provide a plug-and-play, framework-agnostic interactivity layer for HTML tables. It handles:
- **Live Search Filtering:** Real-time search across table records with continuous typing focus.
- **Row Selection:** Detecting row clicks and extracting corresponding record data.
- **Row Actions:** Triggering custom actions (e.g. `show`, `edit`, `delete`) from button clicks within table rows.
- **Footer Save / Create:** Reading user input from table footer fields and creating new records declaratively.

### 3. How to Use
Import `hookTableEvents` and attach it to your container:

```javascript
import { hookTableEvents } from "json-renderers-build-table-events";

// Initial table data
const dataset = [
    { itemName: "High Tensile Bolt", category: "Fasteners", baseUnit: "kgs" },
    { itemName: "Galvanized Wire Coil", category: "Raw Materials", baseUnit: "kgs" }
];

const columns = [
    { columnName: "itemName" },
    { columnName: "category" },
    { columnName: "baseUnit" }
];

// Hook interactivity to table container
hookTableEvents({
    container: document.getElementById("table-container"),
    dataset: dataset,
    columns: columns,
    renderTable: (filteredRecords) => {
        // Re-render only the inner table / tbody to preserve input focus
        renderTableBody(filteredRecords);
    },
    onSearch: (query, filteredRecords) => {
        console.log(`Search query: "${query}" - Found ${filteredRecords.length} records`);
    },
    onRowClick: (record, rowIndex) => {
        console.log(`Clicked row ${rowIndex + 1}:`, record);
    },
    onRowAction: (action, record, rowIndex) => {
        console.log(`Action "${action}" on row ${rowIndex + 1}:`, record);
    },
    onSave: (newRecord, updatedDataset) => {
        console.log("New record saved:", newRecord);
    }
});
```

### 4. How to Build
To build the distribution bundle locally:

```bash
# Clone the repository
git clone https://github.com/keshavsoft/json-renderers-build-table-events.git
cd json-renderers-build-table-events

# Install dependencies
npm install

# Run the build command
npm run build
```
The compiled, minified bundle will be generated in `docs/dist/v1/min.js`.

---

## 📑 Documentation Pages
Explore the dedicated documentation pages:
- 📖 [What is the Purpose & The Story](https://keshavsoft.github.io/json-renderers-build-table-events/pages/purpose.html)
- 💡 [How to Use & API Guide](https://keshavsoft.github.io/json-renderers-build-table-events/pages/how-to-use.html)
- ⚡ [Interactive Live Example](https://keshavsoft.github.io/json-renderers-build-table-events/pages/example.html)
- 🛠️ [How to Build & Development Guide](https://keshavsoft.github.io/json-renderers-build-table-events/pages/how-to-build.html)

---

## 📄 License
MIT © [KeshavSoft](https://github.com/keshavsoft)
