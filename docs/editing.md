# Editing contract

[Manual](../README.md)

Text, MCP and canvas share name resolution and structural validation.
They use the same explicitly selected semantic adapters for typing and interpretation.
This prevents results from depending on the interface used.

## Operations

| Operation | Effect |
| --- | --- |
| Create node | Generate an ID and local name; type is optional. |
| Assert | Create an occurrence with its own ID. |
| Qualify | Assert something about an existing statement. |
| Retract statement | Remove the active occurrence selected by ID. |
| Rename | Preserve the ID and update references. |
| Add to group | Create a membership statement. |

Retracting a statement does not assert its negation.
Removing a referenced entity requires resolving its references; there is no silent cascade.
Replacing a value combines retraction and assertion in one transaction.

Every mutation uses the expected document revision and an idempotency key.
Repeating the same request returns the same result.
A new request may create another occurrence of the same proposition.
A revision mismatch requires reconciliation; it does not allow a silent overwrite.

## Diagnostics and suggestions

Unknown names and invalid syntax prevent the affected transaction from being applied.
With the standard typing adapter enabled, type mismatches produce warnings as defined in [types](types.md).
An invalid text draft may remain in the editor without changing the active graph.

The suggestion service accepts a subject, partial relation or expected value type.
It returns candidates, compatibility and explanations.
The LSP, MCP and canvas connection picker consume this service.

## Knowledge review

Review is an application workflow. Core has no approval states.

AI proposals must preserve their source when available.
Approval requirements depend on the capture workflow, not on the use of MCP.
Pending proposals are excluded from accepted-fact queries.
Rejecting a proposal preserves its record for audit.
Storage for this workflow must be defined before automated ingestion.

## Stable writing

The formatter preserves order, comments, names and IDs.
It normalizes indentation and quote style as defined in [syntax](syntax.md).
It does not sort the entire graph after each edit, because that creates diffs without changes in knowledge.
Textual order does not imply semantic order.
Action sequences will require an explicit representation in a later version.
