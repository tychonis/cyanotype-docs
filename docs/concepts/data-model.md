---
title: Core Data Model
sidebar_position: 2
---

# Core Data Model

Cyanotype represents product definitions using four fundamental entities: **Item, CoItem, Process, and CoProcess**.

Together, they describe what a product requires, how those requirements are satisfied, and how concrete products are constructed.

## 1. Item

An **Item** represents a concrete implementation of a part or assembly.

An Item may contain engineering attributes, part numbers, descriptions, and references to external artifacts such as CAD models, drawings, or specifications.

Items have distinct identities even when they satisfy the same requirements.

For example, two motors manufactured by different suppliers are separate Items, even if either can be used in the same assembly.

An Item can represent both an individual component and a complete assembly.

## 2. CoItem

A **CoItem** represents a set of requirements that an Item must satisfy.

Unlike an Item, which describes a particular implementation, a CoItem describes what is needed without necessarily specifying how that need is fulfilled.

For example, a CoItem might describe a motor mounting position with requirements for torque, dimensions, and electrical characteristics.

Multiple Items may satisfy the same CoItem, enabling interchangeable implementations.

All Items satisfying the same CoItem are interchangeable within that CoItem's context. However, a different CoItem may impose different requirements, and only a subset of those Items may satisfy it.

Thus, interchangeability is guaranteed within a given CoItem, but does not imply universal equivalence between Items.

## 3. Process

A **Process** describes how an Item is constructed from its inputs.

For an assembly, a Process specifies the required input CoItems, their quantities, and potentially their placements and other assembly information.

For example, a linear actuator assembly may require:

- One motor
- One lead screw
- Two bearings
- One housing

Each input is represented by a CoItem, allowing the assembly definition to specify requirements independently of the particular components chosen to satisfy them.

A Process connects the resulting Item to its input CoItems.

A Process may also be **abstract**, representing an engineering relationship between an Item and its input CoItems without specifying an executable manufacturing or assembly procedure. Such Processes describe product structure and dependencies rather than how the Item is physically produced.

## 4. CoProcess

A **CoProcess** represents the qualification relationship through which an Item satisfies a CoItem.

Conceptually, it is analogous to a quality assurance or validation process.

A CoProcess can express the conditions under which a particular implementation is accepted for a particular requirement.

A CoProcess represents qualification, while a Process represents the construction or engineering relationships of an Item. The two represent different relationships.

## 5. Relationships Between Entities

The four entities form a repeating directed structure:

```text
CoItem ← CoProcess ← Item ← Process ← CoItem ...
```

**Process and CoProcess act as explicit, first-class relationships between Item and CoItem.**

- **CoProcess** connects an Item to a CoItem whose requirements it satisfies, establishing qualification and interchangeability.
- **Process** connects an Item to the CoItems it requires as inputs, describing its construction or engineering dependencies.

Representing these relationships as first-class entities allows them to carry their own attributes, identities, and revision histories.

The distinction between the two process types is deliberate.

A **Process** describes how something is made.

A **CoProcess** describes how an implementation is qualified to satisfy a requirement.

## 6. BOMTree

A BOMTree is a resolved, hierarchical view of the product definition graph.

Each BOMTree node contains four associated entities:

```text
CoItem ← CoProcess ← Item ← Process
```

The final relationship, between a Process and its input CoItems, crosses BOMTree node boundaries.

For example, consider an actuator assembly requiring a motor and a gearbox.

```text
Actuator Node
  CoItem A
    ← CoProcess A
    ← Item A
    ← Process A
         ├── CoItem B (Motor)
         │     ← CoProcess B
         │     ← Item B
         │     ← Process B
         │
         └── CoItem C (Gearbox)
               ← CoProcess C
               ← Item C
               ← Process C
```

Here:

- Node A represents the actuator assembly.
- Process A requires CoItems B and C.
- Nodes B and C resolve those requirements into concrete motor and gearbox implementations.
- Each child Item may have its own Process and further input requirements.

A Process may have multiple input CoItems, creating branches in the BOMTree.

The BOMTree is a contextual resolution of the underlying definitions, rather than a requirement that every entity have a unique parent globally.

## 7. Interchangeability

Interchangeability emerges from qualification relationships rather than from treating multiple Items as identical.

Consider two motors, Item M1 and Item M2, that both satisfy CoItem M.

```text
                  CoItem M
                ↗           ↖
           CoProcess 1  CoProcess 2
                ↑           ↑
              Item M1     Item M2
```

Both implementations can be considered candidates for the same requirement.

However, this does not imply that M1 and M2 are universally interchangeable. A different CoItem may impose requirements satisfied by only one of them.

Qualification relationships may also change across revisions as requirements, implementations, or supporting evidence evolve.

By preserving the identities of Items, CoItems, and their qualification relationships, Cyanotype can track interchangeability without losing engineering history.

## 8. Design Principles

The model follows several principles:

1. **Separate requirements from implementations.** CoItems define what is required; Items define concrete solutions.
2. **Separate construction from qualification.** Processes define how Items are constructed; CoProcesses establish how Items satisfy requirements.
3. **Preserve identity.** Distinct implementations remain distinct even when interchangeable in a particular context.
4. **Represent context explicitly.** Interchangeability depends on the requirements being satisfied.
5. **Support recursive composition.** Processes may consume multiple CoItems, allowing assemblies to form hierarchical product structures.
6. **Track evolution.** Definitions and qualification relationships can change independently across revisions.

This separation provides the foundation for version-controlled product definitions, automated configuration, engineering change management, and traceable part substitution.
