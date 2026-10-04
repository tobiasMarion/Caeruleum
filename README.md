# Caeruleum

Caeruleum is a small, human-readable language for building structured knowledge bases while the structure of the domain itself is still being discovered.

Plain-text source, assets and views are persistent. Indexes, embeddings and databases are derived and disposable.

## Core

```text
Entity    := Node | Statement | Group
Value     := Entity | Literal
Statement := Entity Relation Value
Literal   := String | Number
```

### Nodes

```cae
library: ROOM
    +BLUE
    +INDOOR

watering_can: ITEM
```

Missing attributes are unknown, not false.

```cae
+BLUE
-OUTDOOR
```

Classes are themselves referencable entities.

```cae
major_key: MAJOR_KEY
MAJOR_KEY -RELATED_TO-> LOCK
```

### Statements

```cae
watering_can -FOUND_IN-> greenhouse
major_key -OPENS-> door
room -RANK-> rank_5
room -FLOOR-> 5
```

Relations are open-ended, directional and may have one modifier.

```cae
major_key -MIGHT<OPEN>-> door
major_key -NOT<OPEN>-> other_door
```

Statements may be named and referenced.

```cae
@location:
watering_can -FOUND_IN-> greenhouse

@location -SUPPORTED_BY-> screenshot
```

### Properties and literals

```cae
library
    name: "Library"
    floor: 3
    description: """
    A room filled with books.
    """
```

A value with identity is a node; an atomic value is a literal.

### Assets

```cae
screenshot: IMAGE
    @asset "assets/screenshot.png"

letter: DOCUMENT
    @asset "assets/letter.pdf"
```

Assets are referenced by the language but stored externally.

### Groups

Groups are first-class, referencable entities that collect related knowledge without creating a namespace.

```cae
group greenhouse_discovery {
    ref watering_can
    ref screenshot
    ref @location

    greenhouse
        +GREEN
}
```

Groups may participate in the graph like any other entity.

```cae
greenhouse_discovery -SUPPORTED_BY-> screenshot
greenhouse_discovery -RELATED_TO-> watering_can_investigation
```

The same entity, statement or group may belong to multiple groups.

### Modules

Imports and exports follow TypeScript semantics.

```cae
// rooms.cae
export kitchen: ROOM
export library: ROOM
```

```cae
import { kitchen, library } from "./rooms"
import { watering_can as can } from "./items"
import * as rooms from "./rooms"

can -FOUND_IN-> rooms.library
```

References to unknown identifiers create incomplete nodes.

## Layer 2

Higher-level constructs lower to Core entities, statements and groups.

```cae
todo bring_watering_can PENDING {
    "Bring the watering can to the kitchen."

    ref watering_can
    ref kitchen

    goal watering_can -IN-> kitchen
}
```

Additional structure is optional.

```cae
todo test_watering_can PENDING {
    ref watering_can
    ref kitchen

    goal watering_can -USED_IN-> kitchen
    requires player -HAS-> watering_can

    do {
        player -TAKE-> watering_can
        watering_can -MOVE_TO-> kitchen
    }
}
```

Future constructs may include hypotheses and other specialized groups.

## Layer 3

Tooling consumes the semantic model rather than reimplementing Caeruleum.

- VS Code extension
- formatter and diagnostics
- LSP: autocomplete, hover, references, rename and CodeLens
- full-text and structural search
- MCP server for semantic reads and writes
- screenshot, OCR and document ingestion
- human approval of AI-extracted knowledge
- 2D graph editor
- persistent spatial layouts and clusters
- multiple visual instances of the same entity

## Roadmap

### v0.1 — Core

- nodes, statements and groups
- classes and binary attributes
- string and numeric literals
- properties
- relation modifiers
- named statements
- asset references
- TypeScript-style modules, imports and exports
- parser and semantic model
- formatter
- CLI validation
- VS Code syntax highlighting and syntax diagnostics

### v0.2 — Knowledge workflow

- structured TODOs
- optional `goal`, `requires` and `do`
- query API
- textual and structural search
- full LSP
- semantic highlighting
- asset and document search

### v0.3 — Tooling

- MCP server
- AI-assisted capture
- screenshot and OCR ingestion
- 2D graph editor
- persistent layouts and clusters
- visual node clones
- asset previews

### Later

- SQLite index
- embeddings
- hypotheses
- richer graph queries
- pattern discovery
