# Organization and visualization

[Manual](../README.md)

## Groups

```cae
discovery: Group
    member = watering_can
    member = screenshot
    member = @possible_open
```

A group is a node with membership relations.
It may contain nodes and statements. An entity may belong to several groups.
Membership does not imply classification, ownership, a namespace or spatial containment.
This allows knowledge to be reorganized without renaming it.

## Assets

```cae
screenshot: Image
    resource = "assets/screenshot.png"
```

An asset is a node that references an external file.
`resource` resolves paths relative to the source file.
Moving the declaration requires adjusting the path to preserve the resource reference.
Binary content stays outside the graph.

`Group`, `member`, `Image`, `Document`, `resource` and `name` belong to the Core vocabulary.
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
