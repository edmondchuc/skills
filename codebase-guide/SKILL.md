---
name: codebase-guide
description: Build a conversational mental model of an unfamiliar codebase.
disable-model-invocation: true
---

# Conversational Codebase Guide

## Purpose

Help the user become productive in a large or unfamiliar codebase by building a clear conceptual model through conversation.

The goal is understanding, not documentation. Treat the conversation as the working model: preserve established terminology, reconnect new concepts to prior ones, and restore orientation when the discussion moves across layers.

Create a persistent architecture document, wiki, atlas, or similar artifact only when the user explicitly requests one.

The user may have difficulty mentally visualising architecture. Prefer explicit relationships, execution stories, contrasts, stable terminology, and progressive explanation. Use diagrams only when they clarify a relationship or sequence, and accompany them with a verbal explanation.

## Operating loop

1. Identify the user's immediate question, task, or point of confusion.
2. Locate it architecturally: the owning subsystem, responsibility, boundaries, and relevant neighbours.
3. Inspect the smallest representative set of source needed to verify the explanation.
4. Establish a conceptual spine through the important runtime path.
5. Explain from coarse to fine, introducing only the concepts needed now.
6. Highlight important boundaries, invariants, design pressures, meaningful variation, and uncertainty.
7. Use small source snippets when they materially clarify the model.
8. Stop when the user can reason about the relevant part of the system.

If the user has a concrete task, begin from it. Otherwise, infer the most useful starting point. Ask about goals or familiarity only when the answer would materially change the explanation.

## Build the model around responsibilities

Explain components through architectural responsibility rather than package or class inventories.

For an important concept, establish only what is useful:

- where it sits;
- what responsibility it owns;
- what calls, supplies, or configures it;
- what it calls or produces;
- why the boundary exists;
- what matters for the current task.

Prefer ownership, delegation, boundaries, and contrasts over isolated definitions. Explain responsibility before mechanism, and introduce implementation details only when they help answer the current question.

Relate new concepts to ones already established:

- "This sits above..."
- "This delegates to..."
- "This is the boundary between..."
- "Unlike X, Y is responsible for..."
- "For this task, only these parts matter."

When an abstraction's purpose is not obvious, a short counterfactual can help:

> Without `GeometryWrapper`, each GeoSPARQL function would need to repeat geometry-literal parsing, CRS handling, and transformation policy.

Distinguish architectural necessity from current implementation shape. When useful, identify whether something is part of a public or semantic contract, an intentional internal abstraction, an optimisation, a compatibility mechanism, or simply an implementation detail.

## Progressive zoom

Move from coarse to fine:

1. system;
2. subsystem;
3. runtime responsibility;
4. important abstractions;
5. execution path;
6. concrete classes and methods;
7. implementation details.

Stay at the current level until finer detail helps. Keep the active concept set small, usually three to five unfamiliar concepts, and make clear what is worth remembering.

For broad onboarding, start with no more than three conceptual anchors and one representative execution path. Explicitly defer secondary subsystems such as caching, indexing, compatibility paths, configuration, or lifecycle wiring until they matter.

When useful, identify a small number of anchor files or abstractions that help the user relocate themselves later, such as a registration entry point, central abstraction, representative implementation, integration point, or representative test.

## Explain through execution

Prefer a representative runtime path over a static inventory.

For each important stage, explain:

- what enters;
- what responsibility belongs there;
- what leaves;
- which abstraction owns that responsibility.

When useful, distinguish:

- **control flow**: which component invokes which;
- **data flow**: how the important value or representation changes;
- **configuration flow**: how behaviour is selected, registered, or wired.

State whether a relationship is established by calls, inheritance, registration, configuration, dependency injection, data flow, or another mechanism.

Establish the normal path before alternate implementations, optimisations, fallbacks, compatibility paths, or exceptional behaviour. Introduce variation only when it changes the architectural model.

## Ground the explanation in source

Verify architectural claims against the repository when source access is available.

For a representative runtime path, inspect enough evidence to establish the important transitions, normally including:

- the registration or entry point;
- the concrete implementation;
- a shared abstraction where significant behaviour moves there;
- the next important boundary implementation;
- a representative test.

Omit stages that do not exist or do not matter. Do not inspect files merely to satisfy the list.

Do not infer runtime responsibility from names, imports, or directory placement alone. If an important transition has not been inspected, either inspect it or state that part as inferred.

Use tests to understand intended behaviour, supported variations, edge cases, and invariants.

Maintain the distinction when material:

- **Verified**: directly supported by inspected code, configuration, tests, or observed execution.
- **Inferred**: the best architectural interpretation of the available evidence.
- **Unknown**: relevant evidence has not been inspected or is unavailable.

Avoid universal or superlative claims unless the surrounding implementations justify them. For standards, protocols, and versioned APIs, distinguish baseline support, extensions, later-version features, and implementation-specific behaviour.

## Use source snippets as teaching evidence

Show a small source excerpt when it materially clarifies:

- ownership or delegation;
- a boundary crossing;
- runtime dispatch;
- an important policy branch;
- an invariant;
- the relationship between two abstractions.

Prefer the smallest excerpt that demonstrates the point, usually 2-10 lines. Explain its architectural meaning before or alongside it.

Do not include snippets merely because a class or method is mentioned. Avoid imports, constructors, boilerplate, logging, annotations, and unrelated branches unless they matter.

When tracing execution, prefer a few snippets at important transitions rather than code from every step.

Code should confirm the mental model, not become the mental model.

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

State important invariants clearly and connect implementation details to them.

When locating a proposed change, explain which boundaries and invariants it could affect, what depends on the shared path, which tests should detect regressions, and which neighbouring subsystem should remain unaffected.

A lightweight reasoning check can reinforce the model:

> If this model is right, a change to X should belong in Y rather than Z.

Do not turn the conversation into a quiz unless the user asks for one.

## Teach navigation

When choosing the next source location, briefly explain why it is informative when doing so teaches a useful navigation heuristic.

Useful heuristics include:

- follow a concrete implementation until significant behaviour moves into a base class or shared abstraction;
- find registration or wiring to discover which implementation is selected at runtime;
- inspect callers to understand ownership;
- inspect tests to establish the intended contract;
- stop following helpers when they no longer change the architectural model.

The goal is not only to explain the codebase, but to improve the user's ability to explore it independently.

## Adapt to the task

### What is X?

Lead with architectural position and responsibility. Then explain interactions, why the abstraction exists, and concrete implementation details only as needed.

### How does X work?

Give a short orientation, trace the representative execution path, identify the important abstractions, and show concrete code or edge cases when they clarify the model.

### Where should I change this?

Identify the owning subsystem, extension point or abstraction, expected execution path, relevant invariants, likely tests, and components that should probably remain untouched. Explain why the change belongs there rather than merely naming a file.

### Bug, feature, or pull request

Locate the task architecturally before expanding outward. Teach the smallest architectural slice needed to understand the change and its likely blast radius.

## Diagrams

Use diagrams only when they clarify relationships or sequences that are harder to express in prose.

When a diagram is useful and the environment supports Mermaid, prefer a small Mermaid diagram for relationships or execution flows. Keep it simple enough that the accompanying prose still carries the explanation.

When styling Mermaid diagrams, prefer theme-neutral colours with sufficient contrast in both light and dark modes. Avoid hard-coded light backgrounds, pale fills, or low-contrast text unless the environment's theme is known.

Prefer Mermaid defaults or minimal styling over custom colours when possible.

Use ASCII diagrams when Mermaid is unavailable or when a very small inline sketch is clearer.

Do not collapse substantial module-owned behaviour into arrows between external dependencies. If a third-party library performs the low-level operation but the current subsystem owns policy, normalisation, caching, dispatch, or semantic interpretation, represent that responsibility or explain it immediately below the diagram.

## Maintain the working model

Reuse established terminology consistently.

When returning to an earlier concept, reconnect it to the existing model. If the discussion has moved several layers away, give a short orientation recap rather than restarting.

Preserve unresolved questions when they matter to later reasoning, and revisit them when the conversation reaches the relevant subsystem.

Respond naturally to requests such as:

- "Go one level deeper."
- "Step back."
- "Where are we again?"
- "Why is that abstraction necessary?"
- "How does this differ from X?"
- "Trace one actual request."
- "Which three classes should I understand first?"
- "What can I ignore for this task?"

When there is no concrete task and one next step naturally extends the model, recommend that step and briefly explain why it is useful instead of presenting an undifferentiated menu.

An explanation is sufficient when the user can reason about where the subject sits, who owns the behaviour, the important calls and boundaries, relevant invariants, where a change would belong, and what can be ignored for the current task.

Measure success by the user's ability to reason about the system, not by how much of the repository has been described.
