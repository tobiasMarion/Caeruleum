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

These are names used in this specification, not reserved language identifiers.

- A node represents something that can be referenced independently.
- A relation is any node used in the relation position.
- A proposition connects a subject, a relation and a value.
- A statement records an assertion of a proposition. It has its own identity.
- A literal has no identity. The initial literal kinds are strings and numbers.

Two sources may assert the same proposition. Their statements remain distinct to preserve provenance.
Statements about statements use the same mechanism as all other knowledge.

## Semantic neutrality

Core stores structure and identity. It assigns no domain meaning to relation names.
No vocabulary is imported implicitly.
A node named `polarity`, `classType` or `name` has no special Core behavior.

Core does not infer truth, negation, possibility, inheritance or group membership.
It preserves supplied statements, including statements a vocabulary may interpret as conflicting.
An omitted statement supplies no information.

[Libraries](libraries.md) define vocabularies. Semantic adapters implement their interpretation in tools.
The typing adapter can report a mismatch. An assertion adapter can interpret a negative claim.
Neither changes the parser or the structural model.

Names, imports and screen coordinates belong to storage or tooling.
They do not assert domain facts.
