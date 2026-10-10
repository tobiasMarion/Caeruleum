# Model

[Manual](../README.md)

## Core

```text
Entity      := Node | Statement
Value       := Entity | Literal
Relation    := Node
Proposition := Entity Relation Value
Statement   := Assertion of Proposition
Literal     := String | Number
```

- A node represents something that can be referenced independently.
- A relation is a node used to connect a subject to a value.
- A proposition describes that connection.
- A statement records an assertion of a proposition. It has its own identity.
- A literal has no identity. The initial literal types are strings and numbers.

Two sources may assert the same proposition. Their statements remain distinct to preserve provenance.

## Open knowledge

Missing knowledge is unknown. It is not false.

An entity may have several types and several values for a relation.
Conflicting statements are preserved. The system reports the conflict without silently choosing a truth.

## One mechanism

Types, relations, groups and assets are nodes.
Knowledge about them is expressed through statements.
This lets tools query the vocabulary with the same operations used for the rest of the graph.

Names, imports and screen coordinates belong to storage or tooling.
They do not assert domain facts.

## Qualification

Modality, polarity, confidence and source qualify a statement.
They are statements about that entity. The underlying relation keeps its name.

An unqualified statement is an ordinary positive assertion.
Missing confidence is unknown; it does not mean `1`.

A statement marked as possible or negative must not be consumed as a confirmed positive fact.
Queries and inference must preserve this distinction.
