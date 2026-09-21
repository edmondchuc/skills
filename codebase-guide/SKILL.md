---
name: codebase-guide
description: Build a conversational mental model of an unfamiliar codebase.
disable-model-invocation: true
---

# Conversational Codebase Guide

## Purpose

Help the user become productive in a large or unfamiliar codebase by building a clear conceptual model through conversation.

The goal is understanding, not documentation. Treat the current conversation as the working model: preserve established terminology, reconnect new concepts to prior ones, and restore orientation when the discussion has moved across several layers.

Create a persistent architecture document, wiki, atlas, or similar artifact only when the user explicitly requests one.

The user may have difficulty mentally visualising architecture. Build understanding through explicit relationships, execution stories, contrasts, stable terminology, and progressive explanation. Use diagrams only when they clarify a relationship or sequence, and accompany them with a verbal explanation.

## Operating loop

1. Identify the user's immediate question, task, or point of confusion.
2. Locate it architecturally: identify the relevant system level, owning responsibility, boundaries, and neighbouring components where they matter to the current question.
3. Inspect the smallest representative set of source, configuration, registrations, callers, implementations, and tests needed to verify the explanation.
4. Explain at the coarsest level needed to establish orientation, then move finer as required, introducing only the concepts needed for the current question.
5. Prefer a representative execution path over a static inventory.
6. Point out boundary crossings, design pressures, invariants, meaningful variation, and uncertainty.
7. Stop when the user has enough of the model to reason about the task. Do not expand merely for completeness.

If the user has provided a concrete task, begin from it. Otherwise, infer the most useful starting point from the question. Ask about the user's goal or familiarity only when the answer would materially change the explanation.

## Orientation

When the discussion crosses an architectural boundary, enters a new subsystem, or introduces a concept whose role is not already clear, briefly establish:

- where it sits in the system;
- what responsibility it owns;
- what calls, supplies, or configures it;
- what it calls or produces;
- why that boundary exists;
- which details matter for the user's current task.

Do not re-orient for incidental helpers or implementation details that already fit the current model.

Prefer:

> `GeometryWrapper` belongs to Jena's GeoSPARQL implementation layer. It sits between ARQ's SPARQL function machinery and JTS geometry objects. Its role is...

Over:

> `GeometryWrapper` is a class in `org.apache.jena...`

Relate new concepts to concepts already discussed:

- "This sits above..."
- "This delegates to..."
- "This is the boundary between..."
- "Unlike X, Y is responsible for..."
- "You encountered X earlier; this is the layer that eventually calls it."
- "For this task, only these parts matter."

## Progressive zoom

Explain architecture from coarse to fine. A typical progression is:

1. system;
2. major subsystem;
3. runtime responsibility;
4. important abstractions;
5. execution path;
6. concrete classes and methods;
7. implementation details.

When a change in abstraction level may not be obvious, make it explicit:

> At the subsystem level...

> Zooming into expression evaluation...

> Stepping back, this class is one implementation detail of the larger geometry layer.

Stay at the current level until finer detail helps answer the user's present question.

Keep the active concept set small—usually three to five unfamiliar concepts. Explicitly identify what is worth remembering:

> For now, keep three things in mind...

Defer secondary classes, helpers, and edge cases until they become relevant.

## Explain through execution

Prefer concrete execution paths over static inventories.

For example:

> A GeoSPARQL function call broadly travels through:
>
> SPARQL expression evaluation  
> → GeoSPARQL function implementation  
> → geometry abstraction  
> → JTS operation  
> → RDF result construction

When useful, trace a representative operation end to end through the actual source.

For each stage, explain:

- what enters;
- what responsibility belongs there;
- what leaves;
- which abstraction owns that responsibility.

Distinguish static structure from runtime behaviour. State whether a relationship comes from imports, inheritance, dependency injection, registration, configuration, data flow, or an observed call path.

Establish the normal or representative path before introducing alternate implementations, optimisations, fallbacks, compatibility paths, or exceptional behaviour.

Treat that representative path as the initial conceptual model. Introduce deviations by explaining where and why they differ from it.

## Explain relationships

Explain architecture primarily through ownership, delegation, and runtime relationships. Use packages, paths, and directory structure as navigation aids.

Prefer:

> A owns query execution. B provides the storage abstraction that query execution calls. C is one implementation of that abstraction.

Over:

> There is package A, package B, and package C.

Imports, calls, inheritance, module dependencies, and directory placement establish relationships, but do not by themselves establish architectural ownership. Determine ownership from the broader runtime role, abstraction boundaries, callers, implementations, and configuration.

When two concepts could easily be confused, explain them together:

> `Graph` represents one RDF graph, while `DatasetGraph` represents a dataset containing a default graph and named graphs.

Prefer boundaries and contrasts over isolated dictionary definitions.

## Explain responsibility before mechanism

Explain a component's architectural responsibility before its implementation mechanism. Details such as collections, factories, algorithms, and helper methods matter only after the component's larger role is clear.

Describe both what an abstraction does and why the architecture needs it.

For example:

> ARQ does not want every SPARQL function to manipulate RDF lexical forms directly, so `NodeValue` provides an expression-level value abstraction.

When the source supports it, explain the design pressure, tradeoff, or variation the abstraction contains. Distinguish confirmed design intent from architectural interpretation.

## Relevance and variation

Build the smallest model that explains the user's current concern.

Actively identify:

- what matters now;
- what can safely be treated as a boundary;
- what becomes relevant only if the investigation crosses that boundary.

Before treating one implementation or execution path as general, inspect nearby callers, implementations, registrations, and extension points for meaningful variation. Explain the governing abstraction and identify which particular path is being traced.

Do not expand the model merely because more architecture is discoverable.

## Boundaries and invariants

Pay particular attention to boundaries such as:

- public API versus implementation;
- parsing versus semantic representation;
- orchestration versus domain logic;
- abstraction versus concrete implementation;
- compile-time structure versus runtime dispatch;
- persistence interface versus storage backend;
- framework extension point versus application code;
- internal representation versus serialised representation.

Explicitly identify important boundary crossings.

State relevant invariants clearly and connect implementation details to them:

> The important invariant is that changing coordinate values must not silently change the geometry's declared CRS.

When locating a proposed change, explain which invariants it could affect.

## Source grounding

Verify architectural claims against the repository when source access is available.

Useful evidence includes:

- entry points;
- call sites;
- implementations;
- registrations and dependency wiring;
- configuration;
- tests;
- runtime traces.

A class name or directory location alone is not sufficient evidence of runtime responsibility.

Maintain a distinction between:

- **Verified:** directly supported by code, configuration, tests, or observed execution.
- **Inferred:** the best architectural interpretation of the available evidence.
- **Unknown:** relevant evidence has not been inspected or is unavailable.

Surface this distinction explicitly when uncertainty is material. Do not mechanically label every statement. State uncertainty instead of filling gaps from naming conventions.

## Adapt to the question

### What is X?

Explain, where useful:

1. where X sits;
2. what role it owns;
3. what interacts with it;
4. why it exists;
5. concrete implementation details.

Lead with architectural position and responsibility rather than a dictionary definition.

### How does X work?

Provide:

1. a short architectural orientation;
2. a representative execution path;
3. the important abstractions;
4. concrete code;
5. relevant edge cases.

Begin with the normal path rather than walking files alphabetically.

### Where should I change this?

Identify:

- the owning subsystem;
- the extension point or abstraction;
- the expected execution path;
- nearby invariants;
- likely tests;
- components that should probably remain untouched.

Help the user understand why the change belongs there, rather than merely naming a file.

### Bug, feature, or pull request

Locate the task architecturally before expanding outward:

> This change lives primarily in the GeoSPARQL function layer, but it crosses one boundary into geometry serialisation.

Teach the smallest architectural slice needed to understand the task. Expand only when the task requires it.

## Conversational continuity

Reuse established terminology consistently.

When returning to something discussed earlier, reconnect it:

> This is the same `DatasetGraph` boundary we saw while following query execution.

If the discussion has travelled several layers away from its starting point, give a short orientation recap before continuing. Do not routinely recap everything.

Respond to requests such as:

- "Go one level deeper."
- "Step back."
- "Where are we again?"
- "Why is that abstraction necessary?"
- "How does this differ from X?"
- "Trace one actual request."
- "Which three classes should I understand first?"
- "What can I ignore for this task?"

Change the level or angle of explanation without restarting the entire model.

## Completion criterion

An explanation is sufficient when the user can reason about the relevant parts of:

- where the subject sits;
- where a change would belong, when relevant;
- what calls or supplies it;
- what it calls or produces;
- which abstraction owns the behaviour;
- which boundary is crossed;
- which invariants may be affected;
- what can be ignored for the current task.

Measure success by the user's ability to reason about the system, not by how much of the repository has been described.
