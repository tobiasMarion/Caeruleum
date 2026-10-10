# Syntax

[Manual](../README.md)

Indentation defines bodies. The formatter uses four spaces.
Identifiers are case-sensitive. Capitalization does not determine semantic roles.
Examples assume that domain names have been declared or imported.

## Nodes and relations

```cae
library: Room
    name = "Library"
    floor = 3
    entrance = north_door
    entrance = garden_door
```

`library: Room` declares the node and asserts its type.
A bare header, such as `library`, declares an untyped node or reopens an existing node in the module.
Repeating a header reuses the node. It does not create another identity.

Inside a node body, `relation = value` asserts a connection with that node as subject.
`=` adds a statement; it does not replace previous values.
Each occurrence is a distinct statement, even when it repeats the same proposition.

An identifier in a type, relation or value position is a reference.
References may point to later declarations.
Unresolved names produce errors. They do not silently create nodes.

## Statements about statements

```cae
major_key: Key
    @possible_open: opens = door
        modality = possible
        confidence = 0.72
        source = screenshot
```

A body nested beneath a relation assertion uses that statement as its subject.
`@possible_open` names the statement so it can be referenced elsewhere.
The name is optional; persistent identity is required.

```cae
@possible_open
    polarity = negative
```

An `@name` header reopens an existing named statement.
It does not create a statement without a proposition.
`polarity = negative` negates the proposition; it does not delete the statement.

## Lowering

| Surface | Model |
| --- | --- |
| `x: C` | Statement `x instance_of C`. |
| `p = v` inside `x` | Statement `x p v`. |
| Body of statement `s` | Statements with `s` as subject. |
| Untyped node header | Node without a classification statement. |

`Class`, `Relation`, `String`, `Number`, `instance_of`, `subclass_of`, `domain`, `range`, `modality`, `possible`, `polarity`, `negative`, `confidence` and `source` belong to the implicit Core vocabulary.
Core also provides the names listed in [organization](organization.md).
These names are reserved in this version to keep resolution unambiguous.

The initial syntax excludes arrows, `+`/`-` attributes, nested node declarations and implicit creation with `?`.
One everyday form reduces writing decisions and formatting differences.
