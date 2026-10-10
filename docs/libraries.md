# Libraries

[Manual](../README.md)

Libraries export ordinary nodes. Imports are explicit.
Applications select semantic adapters for the vocabularies they use.
Importing declarations makes names available; it does not execute inference rules.

## Proposed standard library

| Module | Vocabulary | Adapter responsibility |
| --- | --- | --- |
| [types](../std/types.cae) | `classType`, `relationType`, `instanceOf`, `subclassOf`, `domain`, `range`, `stringType`, `numberType` | Type compatibility and autocomplete. |
| [annotations](../std/annotations.cae) | `source`, `modality`, `possible`, `polarity`, `negative`, `confidence` | Provenance and assertion qualification. |
| [organization](../std/organization.cae) | `group`, `member`, `document`, `image`, `resource`, `name` | Membership, resource resolution and display labels. |

These modules are ordinary `.cae` files. Their initial declarations are included in this repository.
The adapter contracts are specified in the manual; adapters are not implemented yet.
Custom vocabularies may replace or extend these modules without changing Core.

## Resolution

Vocabulary names are not reserved. Local declarations may use the same spelling.
Within a module, conflicting bindings require an import alias.
Adapters recognize resolved vocabulary identities, never identifier spelling.
Renaming an import therefore preserves its meaning; an unrelated node with the same name does not acquire that meaning.
The stable identity and versioning of library exports must be settled with the persistence format.

## Assertion annotations

With the standard annotations adapter enabled:

- An unqualified statement is an ordinary positive assertion.
- `modality = possible` marks a proposition as possible.
- `polarity = negative` expresses negation without deleting the statement.
- Missing confidence is unknown; it does not mean `1`.
- Conflicting qualifications remain visible and produce a diagnostic.

Queries for accepted positive facts must exclude possible, negative and unresolved conflicting claims.
Without this adapter, tools may inspect the raw graph but must not present it as evaluated truth.
This keeps interpretation explicit and prevents generic tools from silently discarding qualifiers.

## Typing shorthand

`x: c` is optional surface sugar for `x instanceOf c`.
It requires an explicit local binding named `instanceOf`, normally imported from the types library.
The binding resolves like any other relation. Missing bindings produce a diagnostic.
An alias works when imported under that local name.

The parser records the shorthand. Lowering resolves the relation; Core receives an ordinary statement.
The explicit body form `instanceOf = c` produces the same structure.
No type declaration is needed to create a node or use it as a relation.
