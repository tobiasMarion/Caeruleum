# Implementation

[Manual](../README.md)

This manual describes the proposed direction. It does not document implemented features.

## 1. Validate persistence

Define the versioned representation of IDs and their association with declarations.
Cover nodes, library exports, type assertions and metadata, including unnamed occurrences.
Test renaming, moving between files, copying and merge conflicts.
This decision precedes the parser because it determines whether editing preserves identity.

## 2. Implement Core and library adapters

Deliver the parser, module resolution, lowering, formatter and diagnostics.
Implement classification, inheritance, domains and ranges in the typing adapter.
Implement annotation and organization adapters separately from Core.
Define adapter selection and versioned library identities; never dispatch on name spelling.
Validate forward references, multiple types and incomplete knowledge.

## 3. Deliver autocomplete

Implement a shared suggestion service and its LSP integration.
Use a small example with rooms, objects, relations and evidence.
Check suggestion relevance and explanation clarity.

## 4. Integrate MCP and canvas

Implement transactions, revision checks and idempotency.
Verify that equivalent edits produce the same model across all three interfaces.
Persist layouts separately and allow multiple visual instances per entity.

## Later

- Document and image capture, with provenance and review.
- Explicit cardinality and required-field constraints.
- Queries, hypotheses, goals and ordered sequences.
- Textual and semantic search over rebuildable indexes.

These features must preserve the core. New syntax requires a use case that justifies its cost.
