# Syntax

[Manual](../README.md)

## Formatting

Indentation defines bodies. The formatter uses four spaces.
Use lower camelCase for all vocabulary names, including types: `room`, `physicalObject`, `foundIn`.
Identifiers are case-sensitive. Capitalization does not determine semantic roles.
The naming convention is checked by a style diagnostic; the formatter does not rename bindings.
Renaming requires reference updates and collision checks.

Strings and import paths use single quotes.
The parser accepts both quote styles. The formatter emits single quotes and preserves the decoded value.
Escape an apostrophe as `\'` and a backslash as `\\`.
For example, `"Guide's note"` formats as `'Guide\'s note'`.
`//` starts a comment outside a string.

Only module grammar uses keywords: `import`, `export`, `from` and `as`.
These are recognized in module declarations. They are not reserved semantic names.

## Nodes and relations

```cae
library
    name = 'Library'
    floor = 3
    entrance = northDoor
    entrance = gardenDoor
```

Examples assume referenced names are declared or imported.
A bare header declares a node or reopens an existing node in the module.
Repeating a header reuses the node. It does not create another identity.

Inside a node body, `relation = value` asserts a connection with that node as subject.
`=` adds a statement; it does not replace previous values.
Each occurrence is distinct, even when it repeats the same proposition.

References may point to later declarations.
Unresolved names produce errors. They do not silently create nodes.

## Optional classification

```cae
import { instanceOf } from '../std/types'

library: room
```

`room` must be declared or imported separately.
The header declares `library` and lowers to a statement using the explicitly bound `instanceOf` relation.
The [types library](types.md) supplies its classification meaning. Core does not interpret it.

## Statements about statements

```cae
majorKey
    @possibleOpen: opens = door
        modality = possible
        confidence = 0.72
        source = screenshot
```

The nested body uses the statement as its subject.
All relation and value names above are ordinary references.
Their interpretation requires the appropriate vocabulary and adapter.

`@possibleOpen` names the statement for references elsewhere.
The name is optional; persistent identity is required.
An `@possibleOpen` header reopens that existing statement.
It cannot create a statement without a proposition.

## Lowering

| Surface | Model |
| --- | --- |
| Bare node header | Node without additional statements. |
| `p = v` inside `x` | Statement `x p v`. |
| Body of statement `s` | Statements with `s` as subject. |
| `x: c` with `instanceOf` bound | Statement `x instanceOf c`. |

The initial syntax excludes arrows, `+`/`-` attributes, nested node declarations and implicit creation with `?`.
One everyday form reduces writing decisions and formatting differences.
