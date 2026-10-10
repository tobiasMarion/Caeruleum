# Identity and modules

[Manual](../README.md)

## Three concepts

| Concept | Purpose |
| --- | --- |
| Persistent ID | Internal references, MCP, provenance and layouts. |
| Local name, such as `wateringCan` | Writing and name resolution within a module. |
| Label, such as `name = 'Watering can'` | Presentation through the organization adapter. |

Local names are unique within a module. Labels may repeat.
Node and statement IDs are opaque, stable and stored in versioned data.
Statement IDs are not proposition hashes: different sources may assert the same thing.

Renaming through the editor preserves the ID and updates textual references.
Moving a declaration through the editor preserves the ID and adjusts imports.
Moving a card or changing a group does not change names or IDs.

The storage syntax for IDs is still undecided.
Manual examples show only the authoring syntax.
The parser must preserve stored IDs, never regenerate them on each read.

## Modules

Each file defines a module. Exports expose names; imports make references available.

```cae
// rooms.cae
import { instanceOf } from '../std/types'
export greenhouse: room
```

```cae
// items.cae
import { instanceOf } from '../std/types'
import { item, foundIn } from './vocabulary'
import * as rooms from './rooms'

wateringCan: item
    foundIn = rooms.greenhouse
```

The first file must also import or declare `room`.
Named imports support aliases: `import { greenhouse as room } from './rooms'`.

`rooms.` completes module exports. It does not traverse graph relations.
Groups and visual positions do not create namespaces, because they are mutable forms of organization.

## Persistence

Source text, IDs, assets and layouts are persistent data.
Indexes, embeddings and query databases are derived and rebuildable.
No identity may depend solely on a disposable index.
