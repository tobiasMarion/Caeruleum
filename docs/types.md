# Types

[Manual](../README.md)

The standard typing adapter makes suggestions explainable. Its vocabulary uses the graph's own model.
These rules belong to the types library, not to Core.

```cae
import { instanceOf, classType, relationType, subclassOf, domain, range } from '../std/types'

physicalObject: classType
place: classType

item: classType
    subclassOf = physicalObject

room: classType
    subclassOf = place

foundIn: relationType
    domain = physicalObject
    range = place
```

## Rules

| Declaration | Meaning |
| --- | --- |
| `x: c` | `x` is an instance of `c`. |
| `c subclassOf p` in the model | Every instance of `c` is also compatible with `p`. |
| `domain = c` | The relation expects subjects compatible with `c`. |
| `range = c` | The relation expects values compatible with `c`. |

Inheritance is transitive. Inheritance cycles produce a diagnostic.
An entity may have several types. Classification does not copy property values from the class to its instances.

Multiple domains are accepted alternatives. Multiple ranges are also accepted alternatives.
An omitted domain or range leaves that side without a declared restriction.
`stringType` and `numberType` are types-library descriptors for literal values.

## Compatibility

- **Compatible:** a declared or inherited type is accepted by the relation.
- **Unknown:** types are missing or relevant qualifications conflict.
- **Mismatched:** types are known, but none matches the expected type.

A mismatch produces a warning. It does not prove impossibility, because knowledge may be incomplete.
Using a relation never automatically assigns its domain or range types to its participants.
This prevents suggested structure from becoming an invented fact.

The first version does not enforce cardinality, disjoint classes or required fields.
These constraints require explicit rules in a later version.

## Autocomplete

1. Inside a node body, prioritize relations with a compatible domain.
2. After `=`, prioritize values compatible with the declared range.
3. Include entities with unknown types and indicate the uncertainty.
4. Allow searching for other names; flag mismatches.

Explain each suggestion: “`room` is a subtype of `place`.”
Frequent patterns may rank suggestions, but do not create constraints.
Structural inference does not depend on an LLM.
