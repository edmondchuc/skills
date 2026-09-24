---
name: orient-me
description: Build a conversational mental model of an unfamiliar codebase.
disable-model-invocation: true
argument-hint: "What do you want to be oriented in?"
---

# Orient Me

## Purpose

Help the user become productive in a large or unfamiliar codebase by building a clear conceptual model through conversation.

The goal is understanding, not documentation. Treat the conversation as the working model: preserve established terminology, reconnect new concepts to prior ones, and re-orient the user when the discussion moves across layers.

Create a persistent architecture document, wiki, atlas, or similar artifact only when the user explicitly requests one.

The user may have difficulty mentally visualising architecture. Prefer explicit relationships, a traced spine, contrasts, stable terminology, and progressive explanation.

## Operating loop

1. Identify the user's immediate question, task, or point of confusion, and match it to a task shape below.
2. Orient: locate the subject in the architecture by its owning subsystem, responsibility, boundaries, and relevant neighbours.
3. Ground the explanation in the smallest representative set of source that verifies it.
4. Establish the spine.
5. Zoom from coarse to fine, introducing only the concepts needed now.
6. Highlight the boundaries, invariants, design pressures, meaningful variation, and uncertainty that bear on the question.
7. Stop when the user can reason about where the subject sits, who owns the behaviour, the important calls and boundaries, the relevant invariants, where a change would belong, and what can be ignored for the current task.

If the user has a concrete task, begin from it. Otherwise, infer the most useful starting point. Ask about goals or familiarity only when the answer would materially change the explanation.

When there is no concrete task and one next step naturally extends the model, recommend that step and briefly explain why it is useful.

## Task shapes

### What is X?

Lead with architectural position and responsibility. Then explain interactions, why the abstraction exists, and concrete implementation details only as needed.

### How does X work?

Give a short orientation, trace the spine, identify the important abstractions, and show concrete code or edge cases when they clarify the model.

### Where should I change this?

Identify the owning subsystem, the extension point or abstraction, and the spine the change sits on. Then explain which boundaries and invariants the change could affect, what else depends on the shared path, which tests should detect regressions, and which neighbouring subsystems should remain untouched. Explain why the change belongs there rather than merely naming a file.

### Bug, feature, or pull request

Orient the task before expanding outward. Teach the smallest architectural slice needed to understand the change and its likely blast radius.

## Build the model around responsibilities

Explain components through architectural responsibility rather than package or class inventories.

For an important concept, establish only what is useful:

- where it sits;
- what responsibility it owns;
- what calls, supplies, or configures it;
- what it calls or produces;
- why the boundary exists;
- what matters for the current task.

Prefer ownership, delegation, boundaries, and contrasts over isolated definitions. Explain responsibility before mechanism.

Relate new concepts to ones already established:

- "This sits above..."
- "This delegates to..."
- "This is the boundary between..."
- "Unlike X, Y is responsible for..."
- "For this task, only these parts matter."

When an abstraction's purpose is not obvious, a short counterfactual can help:

> Without `RetryPolicy`, each HTTP client would need to repeat backoff timing, idempotency checks, and error classification.

Distinguish architectural necessity from current implementation shape. When useful, identify whether something is part of a public or semantic contract, an intentional internal abstraction, an optimisation, a compatibility mechanism, or simply an implementation detail.

## Progressive zoom

Move from coarse to fine:

1. system;
2. subsystem;
3. runtime responsibility;
4. important abstractions;
5. the spine;
6. concrete classes and methods;
7. implementation details.

Stay at the current level until finer detail helps. Keep the active concept set small, usually three to five unfamiliar concepts, and make clear what is worth remembering.

For broad onboarding, start with no more than three core concepts and one spine. Explicitly defer secondary subsystems such as caching, indexing, compatibility paths, configuration, or lifecycle wiring until they matter.

When useful, name a small number of anchor files or abstractions that help the user relocate themselves later, such as a registration entry point, central abstraction, representative implementation, integration point, or representative test.

## The spine

The spine is one representative execution path through the part of the system in question. Explain through the spine rather than through a static inventory.

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

To ground the spine, inspect enough evidence to establish its important transitions, normally including:

- the registration or entry point;
- the concrete implementation;
- a shared abstraction where significant behaviour moves there;
- the next important boundary implementation;
- a representative test.

Omit stages that do not exist or do not matter.

Do not infer runtime responsibility from names, imports, or directory placement alone. If an important transition has not been inspected, either inspect it or state that part as inferred.

Use tests to understand intended behaviour, supported variations, edge cases, and invariants.

Maintain the distinction when material:

- **Verified**: directly supported by inspected code, configuration, tests, or observed execution.
- **Inferred**: the best architectural interpretation of the available evidence.
- **Unknown**: relevant evidence has not been inspected or is unavailable.

Make universal or superlative claims only when the surrounding implementations justify them. For standards, protocols, and versioned APIs, distinguish baseline support, extensions, later-version features, and implementation-specific behaviour.

## Use source snippets as teaching evidence

Show a small source excerpt when it materially clarifies:

- ownership or delegation;
- a boundary crossing;
- runtime dispatch;
- an important policy branch;
- an invariant;
- the relationship between two abstractions.

Prefer the smallest excerpt that demonstrates the point, usually 2-10 lines, and explain its architectural meaning before or alongside it. When tracing the spine, show a few snippets at important transitions rather than code from every step.

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

A prediction the model makes can reinforce it:

> If this model is right, a change to X should belong in Y rather than Z.

State the prediction yourself, as a check the user can hold their own understanding against.

## Teach navigation

When choosing the next source location, briefly explain why it is informative when doing so teaches a useful navigation heuristic.

Useful heuristics include:

- follow a concrete implementation until significant behaviour moves into a base class or shared abstraction;
- find registration or wiring to discover which implementation is selected at runtime;
- inspect callers to understand ownership;
- inspect tests to establish the intended contract;
- stop following helpers when they no longer change the architectural model.

The goal is not only to explain the codebase, but to improve the user's ability to explore it independently.

## Diagrams

Use a small diagram only when it clarifies a relationship or sequence that is harder to express in prose, and let the accompanying prose still carry the explanation.

Draw diagrams in Mermaid when the environment renders it. Use ASCII only when Mermaid is unavailable or a tiny inline sketch is clearer.

Keep Mermaid on its default theme and styling, so the renderer can match the viewer's light or dark mode. When colour must carry meaning, choose colours that keep text and edges high-contrast in both light and dark mode.

When a third-party library performs the low-level operation but the current subsystem owns policy, normalisation, caching, dispatch, or semantic interpretation, show that responsibility as its own element in the diagram or explain it immediately below.

## Maintain the working model

Reuse established terminology consistently.

When returning to an earlier concept, reconnect it to the existing model. If the discussion has moved several layers away, re-orient the user with a short recap rather than restarting.

Preserve unresolved questions when they matter to later reasoning, and revisit them when the conversation reaches the relevant subsystem.
