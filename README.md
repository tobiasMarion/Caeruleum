# Caeruleum

Caeruleum is a small, human-readable language for building structured knowledge bases while the structure of the domain itself is still being discovered.

Core stores nodes and statements. Libraries give them meaning.
The typing library describes entities and guides connections and autocomplete.
Text, MCP and a 2D canvas share the same model.

Plain-text source, assets and views are persistent. Indexes, embeddings and databases are derived and disposable.

```cae
wateringCan: item
    foundIn = greenhouse

greenhouse: room
    name = 'Greenhouse'
```

This example assumes an explicit `instanceOf` binding and a vocabulary containing `item`, `room`, `foundIn` and `name`.

## Manual

1. [Model](docs/model.md) — entities, values and assertions.
2. [Types](docs/types.md) — structure, compatibility and autocomplete.
3. [Syntax](docs/syntax.md) — writing knowledge.
4. [Identity](docs/identity.md) — names, modules and persistence.
5. [Organization](docs/organization.md) — groups, assets and layouts.
6. [Editing](docs/editing.md) — a shared contract for text, MCP and canvas.
7. [Libraries](docs/libraries.md) — explicit vocabularies and semantic adapters.
8. [Implementation](docs/roadmap.md) — milestones and open decisions.

## Example

[MAZE: the entrance](examples/maze/README.md) — typed rooms, sourced connections and a tentative interpretation.

**Status:** language proposal. This repository does not yet contain an implementation.
