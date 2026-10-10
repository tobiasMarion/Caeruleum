# Organization and visualization

[Manual](../README.md)

The following meanings require the standard organization adapter.
Examples assume explicit imports from `std/organization` and `std/types`.

## Groups

```cae
discovery: group
    member = wateringCan
    member = screenshot
    member = @possibleOpen
```

A group is a node with membership relations.
It may contain nodes and statements. An entity may belong to several groups.
Membership does not imply classification, ownership, a namespace or spatial containment.
This allows knowledge to be reorganized without renaming it.

## Assets

```cae
screenshot: image
    resource = 'assets/screenshot.png'
```

An asset is a node that references an external file.
`resource` resolves paths relative to the source file.
Moving the declaration requires adjusting the path to preserve the resource reference.
Binary content stays outside the graph.

`group`, `member`, `image`, `document`, `resource` and `name` belong to the organization library.
`member` accepts any entity. `resource` and `name` accept strings.

## Layouts

Layouts store positions, sizes, styles and visual instances by ID.
An entity may have several visual instances, including within the same view.
Editing its knowledge updates all of them.

Dragging a card changes the layout. It does not assert membership or location.
Creating membership requires an explicit semantic operation.

A statement may appear as a connection or a card.
Its identity supports attaching evidence and metadata in either representation.
The visual representation does not create another statement.
