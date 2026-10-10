# Types

[Manual](../README.md)

Types make suggestions explainable. The vocabulary uses the graph's own model.

```cae
PhysicalObject: Class
Place: Class

Item: Class
    subclass_of = PhysicalObject

Room: Class
    subclass_of = Place

found_in: Relation
    domain = PhysicalObject
    range = Place
```

## Rules

| Declaration | Meaning |
| --- | --- |
| `x: C` | `x` is an instance of `C`. |
| `C subclass_of P` in the model | Every instance of `C` is also compatible with `P`. |
| `domain = C` | The relation expects subjects compatible with `C`. |
| `range = C` | The relation expects values compatible with `C`. |

Inheritance is transitive. Inheritance cycles produce a diagnostic.
An entity may have several types. Classification does not copy property values from the class to its instances.

Multiple domains are accepted alternatives. Multiple ranges are also accepted alternatives.
An omitted domain or range leaves that side without a declared restriction.
`String` and `Number` are Core descriptors for literal values.

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

Explain each suggestion: “`Room` is a subtype of `Place`.”
Frequent patterns may rank suggestions, but do not create constraints.
Structural inference does not depend on an LLM.
