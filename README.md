# Caeruleum

Caeruleum is a small, human-readable language for building structured knowledge bases while the structure of the domain itself is still being discovered.

Plain-text source, assets and views are persistent. Indexes, embeddings and databases are derived and disposable.

## Core

Caeruleum has a deliberately small semantic core.

```text
Entity      := Node | Statement
Value       := Entity | Literal
Relation    := Node
Proposition := Entity Relation Value
Statement   := Assertion of Proposition
Literal     := String | Number
```

Anything with identity is a `Node` or a `Statement`.

Everything known about an entity is expressed through statements.

Classes, attributes, properties, groups, assets and higher-level constructs are surface-language conveniences that lower to this core model.

A `Proposition` describes structural knowledge:

```text
watering_can FOUND_IN greenhouse
```

A `Statement` is an occurrence asserting that proposition and has its own identity.

Two statements may therefore assert the same proposition while having different provenance, confidence or other metadata.

---

## Nodes

Nodes are entities with identity.

```cae
library: ROOM
    +BLUE
    +INDOOR

watering_can: ITEM
```

Class declarations are syntactic sugar for statements.

Conceptually:

```cae
library: ROOM
```

lowers to:

```text
library INSTANCE_OF ROOM
```

Classes are ordinary nodes and may themselves participate in statements.

```cae
major_key: MAJOR_KEY

MAJOR_KEY -RELATED_TO-> LOCK
ROOM -RELATED_TO-> BUILDING
```

Relations are also ordinary nodes used in the relation position of a proposition.

This allows knowledge about relations themselves.

```cae
FOUND_IN -INVERSE_OF-> CONTAINS
OPENS -RELATED_TO-> UNLOCKS
```

The language does not impose a closed set of classes, attributes or relations.

---

## Attributes

Binary attributes are shorthand for statements.

```cae
library
    +BLUE
    +INDOOR
    -OUTDOOR
```

Positive and negative knowledge are distinct from missing knowledge.

```cae
+BLUE
-OUTDOOR
```

means that `BLUE` is known to hold and `OUTDOOR` is known not to hold.

An omitted attribute is unknown, not false.

Attributes do not introduce a separate semantic mechanism. They lower to statements in the same semantic model.

---

## Statements

Statements assert propositions between entities and values.

```cae
watering_can -FOUND_IN-> greenhouse
major_key -OPENS-> door
room -RANK-> rank_5
room -FLOOR-> 5
```

Relations are open-ended and directional.

Statements may be named.

```cae
@location:
watering_can -FOUND_IN-> greenhouse
```

Named statements are first-class entities and may participate in other statements.

```cae
@location -SUPPORTED_BY-> screenshot
```

This makes provenance, evidence, uncertainty and other knowledge about assertions explicit without changing the underlying relation.

### Statement metadata

Properties of an assertion belong to the statement, not to its relation.

Instead of creating modified relations such as:

```text
MIGHT<OPEN>
NOT<OPEN>
```

the underlying proposition remains unchanged.

```cae
@possible_open:
major_key -OPEN-> door

@possible_open
    modality: MIGHT
```

Negation is independent from modality.

```cae
@not_open:
major_key -OPEN-> other_door

@not_open
    polarity: NEGATIVE
```

Additional dimensions may be attached independently.

```cae
@possible_open
    modality: MIGHT
    confidence: 0.72
```

This allows future metadata such as provenance, temporal validity, confidence, inferred status or disagreement without encoding additional semantics into relation names.

---

## Properties and literals

Properties provide compact syntax for statements whose value is usually a literal.

```cae
library
    name: "Library"
    floor: 3
    description: """
    A room filled with books.
    """
```

Conceptually:

```cae
library
    floor: 3
```

lowers to:

```text
library FLOOR 3
```

A value with identity is a node.

An atomic value is a literal.

```text
Node    → something that can be independently referenced
Literal → an atomic value such as a string or number
```

Properties are therefore syntax, not a separate semantic primitive.

---

## Assets

Assets are ordinary nodes associated with externally stored resources.

```cae
screenshot: IMAGE
    @asset "assets/screenshot.png"

letter: DOCUMENT
    @asset "assets/letter.pdf"
```

The files themselves are not embedded into the semantic model.

The source stores only their references.

Asset nodes may participate in statements like any other node.

```cae
@location -SUPPORTED_BY-> screenshot
letter -DESCRIBES-> greenhouse
```

`@asset` is source-level metadata used by tooling to resolve the external resource.

---

## Groups

Groups collect related knowledge without introducing a namespace.

```cae
group greenhouse_discovery {
    ref watering_can
    ref screenshot
    ref @location

    greenhouse
        +GREEN
}
```

A group has identity and may be referenced.

```cae
greenhouse_discovery -SUPPORTED_BY-> screenshot
greenhouse_discovery -RELATED_TO-> watering_can_investigation
```

Semantically, however, a group is not a separate kind of entity.

A group lowers to an ordinary node plus membership statements.

Conceptually:

```cae
group greenhouse_discovery {
    ref watering_can
    ref screenshot
}
```

lowers to something equivalent to:

```text
greenhouse_discovery INSTANCE_OF GROUP
greenhouse_discovery MEMBER watering_can
greenhouse_discovery MEMBER screenshot
```

Entities declared inside a group also belong to that group.

```cae
group greenhouse_discovery {
    greenhouse
        +GREEN
}
```

does not create `greenhouse_discovery.greenhouse`.

It declares the ordinary `greenhouse` entity and associates it with the group.

The same node or statement may belong to multiple groups.

Groups organize knowledge semantically.

They do not affect name resolution.

---

## Incomplete nodes

Knowledge capture often refers to entities before their structure is known.

Incomplete nodes are therefore supported explicitly.

```cae
watering_can -FOUND_IN-> ?greenhouse
```

`?greenhouse` declares that the entity is intentionally incomplete.

It may later be completed normally.

```cae
greenhouse: ROOM
    +GREEN
```

A reference to an undeclared identifier without `?` produces a diagnostic rather than silently creating an entity.

This preserves typo detection while still supporting incremental knowledge discovery.

For example:

```cae
watering_can -FOUND_IN-> greenhosue
```

may produce:

```text
error: unknown identifier `greenhosue`
```

---

## Modules

Modules control source organization and name resolution.

They do not add knowledge to the semantic graph.

Imports and exports follow TypeScript-like semantics.

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

Modules create lexical namespaces.

Groups do not.

This distinction is intentional:

```text
Module → source organization and name resolution
Group  → semantic organization of knowledge
```

---

## Semantic lowering

Surface constructs lower to the same small semantic model.

Examples:

```text
library: ROOM
    ↓
library INSTANCE_OF ROOM
```

```text
library
    floor: 3
    ↓
library FLOOR 3
```

```text
library
    +BLUE
    ↓
attribute statement about library and BLUE
```

```text
group discovery {
    ref library
}
    ↓
discovery INSTANCE_OF GROUP
discovery MEMBER library
```

The exact internal representation is an implementation detail, but all constructs ultimately produce nodes and statements.

This keeps the semantic model independent from the surface syntax.

---

# Layer 2

Layer 2 introduces structured domain constructs.

These constructs do not extend the semantic kernel. They lower to Core nodes and statements.

## TODOs

```cae
todo bring_watering_can PENDING {
    "Bring the watering can to the kitchen."

    ref watering_can
    ref kitchen

    goal watering_can -IN-> kitchen
}
```

A TODO has identity and may therefore participate in the graph.

```cae
bring_watering_can -RELATED_TO-> greenhouse_discovery
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

`goal`, `requires` and `do` describe different semantic roles within the TODO but ultimately lower to ordinary entities and statements.

### Ordered actions

Statements in the semantic graph are unordered by default.

A `do` block, however, represents an ordered sequence of intended actions.

```cae
do {
    player -TAKE-> watering_can
    watering_can -MOVE_TO-> kitchen
}
```

The semantic lowering must therefore preserve this order explicitly, for example through step entities or ordering relations.

Conceptually:

```text
step_1 ACTION @take
step_2 ACTION @move

step_1 NEXT step_2
```

The exact generated representation is internal to the semantic model.

The important invariant is that no ordering information from the source construct is lost during lowering.

---

## Future constructs

Additional Layer 2 constructs may include:

- hypotheses
- observations
- questions
- procedures
- experiments
- claims

Like TODOs and groups, these constructs should introduce domain semantics without requiring new fundamental entity kinds whenever they can be represented through nodes and statements.

---

# Layer 3

Tooling consumes the semantic model rather than reimplementing Caeruleum semantics.

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

Views and spatial layouts are persistent user data but are separate from the knowledge graph itself.

Indexes, embeddings and databases remain derived and disposable.

---

# Design principles

## One semantic mechanism for knowledge

All knowledge is represented by statements.

Classes, properties and attributes are convenient ways of writing statements rather than independent semantic systems.

## Identity is explicit

Anything that must be independently referenced has identity.

Values without identity are literals.

## Assertions are first-class

Statements have their own identity.

Different statements may assert the same proposition while differing in provenance, confidence, modality or other metadata.

## Relations remain relations

Negation, uncertainty, confidence, provenance and temporal information qualify assertions rather than producing increasingly specialized relation names.

## Organization is orthogonal to knowledge

Modules organize source code.

Groups organize knowledge.

Layouts organize visualization.

None of these concepts should implicitly perform the work of another.

## Higher layers lower to Core

New domain constructs should normally compile into existing Core concepts rather than expand the semantic kernel.

The Core should remain small even as the language grows.

---

# Roadmap

## v0.1 — Core

- nodes and statements
- propositions and statement identity
- relations as referencable nodes
- classes as surface syntax
- binary attributes as surface syntax
- string and numeric literals
- properties as surface syntax
- statement metadata
- named statements
- groups lowered to nodes and membership statements
- asset references
- explicit incomplete nodes
- TypeScript-style modules, imports and exports
- parser and semantic model
- semantic lowering
- formatter
- CLI validation
- unknown-reference diagnostics
- VS Code syntax highlighting and syntax diagnostics

## v0.2 — Knowledge workflow

- structured TODOs
- optional `goal`, `requires` and ordered `do`
- query API
- textual and structural search
- full LSP
- semantic highlighting
- asset and document search
- statement provenance and confidence
- inspection of lowered semantic structures

## v0.3 — Tooling

- MCP server
- AI-assisted capture
- screenshot and OCR ingestion
- human approval workflow
- 2D graph editor
- persistent layouts and clusters
- visual node clones
- asset previews

## Later

- SQLite index
- embeddings
- hypotheses and specialized Layer 2 constructs
- temporal statement metadata
- richer graph queries
- pattern discovery
- inference over relation semantics
