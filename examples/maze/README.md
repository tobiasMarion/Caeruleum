# MAZE: the entrance

[Caeruleum manual](../../README.md)

A small knowledge base about *MAZE: Solve the World's Most Challenging Puzzle* by Christopher Manson.
It covers Room 1 and its four destinations. It includes a proposed first move, so it contains a minor puzzle spoiler.

## Files

- [vocabulary.cae](vocabulary.cae) defines types and relation signatures.
- [sources.cae](sources.cae) names the evidence used by statements.
- [maze.cae](maze.cae) records rooms, connections and an interpretation.

Read `maze.cae` first. Follow its imports to inspect the structure behind the knowledge.
These files use the proposed authoring syntax. No parser or autocomplete implementation exists yet, and persistent ID syntax remains undecided.

The example explicitly imports the [standard library](../../docs/libraries.md).
The behaviors below require its typing, annotations and organization adapters.
Core itself assigns no meaning to these imported names.

## Reading the graph

Room 1 has exits to Rooms 20, 21, 26 and 41, as listed in the [Room 1 transcription](https://mazecast.wikidot.com/room-1).
Each exit statement references that source.

`leadsTo` records a directed connection. It does not imply a return connection or a recommended move.
The other four rooms have no recorded exits in this example. Their exits are unknown here, not absent in the book.

`preferredExit` records an interpretation. The [community solution commentary](https://mazecast.wikidot.com/room-1) selects Room 26.
This base deliberately keeps that recommendation qualified as `possible`: a reader has captured the interpretation without accepting it as settled.
The source reports a solution; the uncertainty belongs to this example's investigation.

Both source nodes reference the same page. They distinguish its transcription from its commentary.
The investigation group includes a room and two statements, because an investigation can collect evidence and claims as well as places.

## Expected autocomplete

| Cursor position | Expected behavior |
| --- | --- |
| Inside `room1` | Suggest `pageNumber`, `leadsTo` and `preferredExit`. |
| After `leadsTo =` | Prioritize the five rooms: `room` inherits from `place`. |
| After `pageNumber =` | Expect a number. |
| After `sources.` | List `room1Page` and `room1Interpretation`. |

These are expected behaviors, not executable editor demonstrations.
Compatibility alone does not prove a connection exists. A room can be a valid suggestion without being an actual destination.

## Extend the investigation

Read another room. Add its observed exits with a source on each statement.
Declare newly encountered rooms before describing their contents; a type and a name are enough to start.
Keep route interpretations qualified until the investigation accepts them.

Book identification follows [Al Sweigart's web edition](https://inventwithpython.com/mazewebsite/index.html).
The example stores factual connections and source links. Illustrations and narrative passages remain in the referenced editions.
