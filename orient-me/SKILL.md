---
name: orient-me
description: Build a conversational mental model of an unfamiliar or complex system.
disable-model-invocation: true
argument-hint: "What system area, task, or point of confusion?"
---

# Orient Me

## Purpose

Help the user understand and reason effectively about a large, unfamiliar, or complex system by building a clear conceptual model through conversation.

The goal is understanding, not documentation. Treat the conversation as the working model: preserve established terminology, reconnect new concepts to prior ones, and re-orient the user when the discussion moves across layers.

Create a persistent architecture document, wiki, atlas, map, or similar artifact only when the user explicitly requests one.

The user may have difficulty mentally visualising complex systems. Prefer explicit relationships, a traced spine, contrasts, stable terminology, and progressive explanation.

This skill has supported system profiles, but the core method is domain-neutral. If no supported profile matches, infer the system's important actors, responsibilities, boundaries, flows, evidence, and invariants from the available material and apply the core method directly.

## Operating loop

1. Identify the user's immediate question, task, or point of confusion, and match it to a task shape below.
2. Orient: locate the subject in the system by its owning actor or subsystem, responsibility, boundaries, and relevant neighbours.
3. Ground the explanation in the smallest representative set of evidence that verifies it.
4. Establish the spine.
5. Zoom from coarse to fine, introducing only the concepts needed now.
6. Highlight the boundaries, invariants, design pressures, authority, meaningful variation, and uncertainty that bear on the question.
7. Stop when the user can reason about where the subject sits, who owns the relevant responsibility, the important transitions and boundaries, the relevant invariants, where a decision or change belongs, and what can be ignored for the current task.

If the user has a concrete task, begin from it. Otherwise, infer the most useful starting point. Ask about goals or familiarity only when the answer would materially change the explanation.

When there is no concrete task and one next step naturally extends the model, recommend that step and briefly explain why it is useful.

## Task shapes

### What is X?

Lead with position in the system and responsibility. Then explain interactions, why the concept exists, and concrete details only as needed.

### How does X work?

Give a short orientation, trace the spine, identify the important actors or abstractions, and show concrete evidence or edge cases when they clarify the model.

### Where does this belong or how can this change?

Identify the owning actor or subsystem, the relevant decision point or extension point, and the spine the change sits on. Then explain which boundaries and invariants the change could affect, what else depends on the shared path, what evidence would reveal regressions or unintended effects, and which neighbouring parts should remain untouched. Explain why the change belongs there rather than merely naming a location.

### Problem, change, event, or decision

Orient the task before expanding outward. Teach the smallest slice of the system needed to understand the issue and its likely consequences or blast radius.

## Build the model around responsibilities

Explain parts of the system through responsibility rather than inventories of names, components, documents, teams, or files.

For an important concept, establish only what is useful:

- where it sits;
- what responsibility it owns;
- what authorises, calls, supplies, configures, or constrains it;
- what it calls, produces, decides, changes, or records;
- why the boundary exists;
- what matters for the current task.

Prefer ownership, delegation, authority, boundaries, and contrasts over isolated definitions. Explain responsibility before mechanism.

Relate new concepts to ones already established:

- "This sits above..."
- "This delegates to..."
- "This is the boundary between..."
- "Unlike X, Y is responsible for..."
- "For this task, only these parts matter."

When a concept's purpose is not obvious, a short counterfactual can help:

> Without this boundary, each participant would need to make the same decision independently.

Distinguish structural necessity from current implementation or operating practice. When useful, identify whether something is part of a formal contract, governing rule, public or semantic interface, intentional internal abstraction, optimisation, compatibility mechanism, operational convention, or merely an implementation detail.

## Progressive zoom

Move from coarse to fine:

1. system;
2. subsystem or domain;
3. responsibility or authority;
4. important actors, abstractions, or concepts;
5. the spine;
6. concrete processes, interfaces, documents, classes, or decisions;
7. implementation or operational details.

Stay at the current level until finer detail helps. Keep the active concept set small, usually three to five unfamiliar concepts, and make clear what is worth remembering.

For broad onboarding, begin with one or two plain-language sentences explaining what the system does and the problem it solves. Establish system-specific terminology by pairing each name with the concept it represents.

Start with no more than three core concepts and one spine. When a concept is difficult to understand, prefer a concrete end-to-end example through the spine; when an analogy helps, map it back to the actual actors, components, and relationships. Explicitly defer secondary mechanisms until they matter.

When useful, name a small number of anchors that help the user relocate themselves later, such as a governing rule, central actor, entry point, representative process, key document, integration point, source file, or representative test.

## The spine

The spine is one representative path through the part of the system in question, such as an execution path, decision path, data flow, authority path, transaction, workflow, or configuration path. Explain through the spine rather than through a static inventory.

For each important stage, explain:

- what enters or triggers it;
- what responsibility belongs there;
- who or what owns that responsibility;
- what leaves, changes, or gets recorded;
- what boundary is crossed.

When useful, distinguish:

- **action flow**: what happens next and what triggers it;
- **information flow**: how information or representations move and change;
- **authority flow**: who may decide, approve, direct, constrain, or override;
- **resource flow**: how money, goods, capacity, or other resources move;
- **configuration or policy flow**: how behaviour is selected, registered, constrained, or governed.

Use domain-specific flow names when they are clearer. In software, for example, control flow, data flow, and configuration flow are often the most useful distinctions.

State what establishes each relationship: calls, delegation, law, policy, contract, ownership, registration, configuration, dependency injection, workflow, data flow, approval, or another mechanism.

Establish the normal path before alternate implementations, exceptions, optimisations, fallbacks, compatibility paths, appeals, or exceptional behaviour. Introduce variation only when it changes the mental model.

## Ground the explanation in evidence

Verify important claims against the strongest available evidence.

Prefer primary or authoritative evidence where the distinction matters. Depending on the system, useful evidence may include:

- governing rules, legislation, standards, policies, contracts, constitutions, or specifications;
- source code, configuration, schemas, and tests;
- process documents, decision records, minutes, correspondence, or audit trails;
- observed runtime or operational behaviour;
- transaction, event, or activity records.

To ground the spine, inspect enough evidence to establish its important transitions. Do not infer responsibility, authority, or actual behaviour from names, titles, directory placement, or organisational charts alone.

Distinguish formal design from observed practice. A rule, contract, specification, or policy may define how the system is meant to operate while records or observed behaviour show how it actually operates. Explain discrepancies instead of silently treating one as proof of the other.

Maintain the distinction when material:

- **Authoritative**: established by a governing rule, specification, contract, policy, or other source that formally defines the system.
- **Verified**: directly supported by inspected evidence or observed behaviour.
- **Inferred**: the best interpretation of the available evidence.
- **Unknown**: relevant evidence has not been inspected or is unavailable.
- **Contested**: relevant participants or authorities materially disagree about the interpretation.

Use only the distinctions that help the current question. Do not turn every explanation into an evidence taxonomy.

Make universal or superlative claims only when the surrounding evidence justifies them. For standards, protocols, laws, policies, and versioned systems, distinguish baseline rules, extensions, later-version changes, local variations, and implementation-specific behaviour.

## Use evidence as teaching support

Show a small excerpt, example, record, or source reference when it materially clarifies:

- ownership, authority, or delegation;
- a boundary crossing;
- dispatch, routing, or approval;
- an important policy branch;
- an invariant;
- the relationship between two actors or abstractions.

Prefer the smallest evidence that demonstrates the point and explain its meaning before or alongside it. When tracing the spine, show evidence at important transitions rather than overwhelming the user with every available source.

Evidence should confirm the mental model, not become the mental model.

## Boundaries and invariants

Pay particular attention to boundaries such as:

- authority versus administration;
- policy or contract versus implementation;
- public interface versus internal implementation;
- ownership versus delegation;
- orchestration versus domain logic;
- formal rule versus operational practice;
- representation versus underlying meaning;
- responsibility versus execution;
- persistence or recordkeeping versus the process that produced the record.

State important invariants clearly and connect concrete details to them.

A prediction the model makes can reinforce it:

> If this model is right, this decision should belong with X rather than Y.

State the prediction yourself, as a check the user can hold their own understanding against.

## Teach navigation

When choosing the next source of evidence or part of the system, briefly explain why it is informative when doing so teaches a useful navigation heuristic.

Useful general heuristics include:

- follow one concrete case end to end;
- identify who owns the decision or responsibility before inspecting implementation details;
- look for the rule, contract, registration, or wiring that establishes a relationship;
- inspect records or examples to see how the formal model behaves in practice;
- inspect neighbouring cases to understand meaningful variation;
- stop following details when they no longer change the mental model.

The goal is not only to explain the system, but to improve the user's ability to explore it independently.

## Supported systems

Supported system profiles sharpen the core method for domains where useful evidence, terminology, and navigation patterns are predictable. They are optional specialisations, not prerequisites. If no profile matches, use the core method directly.

### Software

For software systems and codebases:

- Treat repositories, packages, modules, services, and runtime components as system parts, but explain them through responsibility rather than inventory.
- For progressive zoom, a useful default is: system -> subsystem -> runtime responsibility -> important abstractions -> spine -> concrete classes and methods -> implementation details.
- For the spine, distinguish control flow, data flow, and configuration flow when useful.
- Ground architectural claims in the repository when source access is available. Inspect enough evidence to establish important transitions, normally including the registration or entry point, concrete implementation, shared abstraction where significant behaviour moves there, next important boundary implementation, and a representative test.
- Do not infer runtime responsibility from names, imports, or directory placement alone.
- Use tests to understand intended behaviour, supported variations, edge cases, and invariants.
- Useful navigation heuristics include following a concrete implementation until behaviour moves into a shared abstraction, finding registration or wiring to discover runtime selection, inspecting callers to understand ownership, and inspecting tests to establish the intended contract.
- Useful boundaries include public API versus implementation, parsing versus semantic representation, orchestration versus domain logic, abstraction versus concrete implementation, compile-time structure versus runtime dispatch, persistence interface versus storage backend, framework extension point versus application code, and internal versus serialised representation.
- Show small source excerpts when they materially clarify ownership, delegation, boundary crossings, runtime dispatch, policy branches, invariants, or abstraction relationships.
- Code should confirm the mental model, not become the mental model.

For software-specific change questions, identify the owning subsystem, extension point or abstraction, shared path, likely blast radius, relevant tests, and neighbouring subsystems that should remain untouched.

### Governance and organisational systems

For governance, administrative, institutional, association, corporate, committee, property-management, and similar systems:

- Begin with the main actors, what authority or responsibility each holds, and how those responsibilities relate.
- Distinguish the system itself from people or agents acting on its behalf. A manager, administrator, committee, officer, contractor, or service provider may execute or administer a function without owning the underlying authority.
- Trace representative decision or issue-resolution paths, such as: issue arises -> responsible body or actor identified -> authority checked -> decision or approval made -> action carried out -> outcome recorded and communicated.
- Pay special attention to authority flow, delegation, decision thresholds, expenditure or resource limits, recordkeeping, escalation, review, and exceptions.
- Ground the model in governing instruments and actual records where available, such as legislation, regulations, constitutions, by-laws, policies, contracts, delegations, meeting minutes, resolutions, correspondence, invoices, and operational records.
- Separate formal authority from documented practice. If participants routinely act differently from what the governing rules appear to require, present both and identify the discrepancy.
- Useful boundaries include collective body versus committee or executive, decision-making versus administration, owner/member responsibility versus shared responsibility, internal governance versus external contractor obligations, formal resolution versus informal agreement, and authority versus implementation.
- When the system is jurisdiction-specific or legally regulated, verify current governing rules rather than assuming that a generic governance pattern applies.

This profile should remain generic enough to cover examples such as a body corporate, owners corporation, strata scheme, association, board, committee, or similar governed organisation. Use the system's own legal and organisational terminology once established.

## Diagrams

Use a small diagram only when it clarifies a relationship or sequence that is harder to express in prose, and let the accompanying prose still carry the explanation.

Draw diagrams in Mermaid when the environment renders it. Use ASCII only when Mermaid is unavailable or a tiny inline sketch is clearer.

Keep Mermaid on its default theme and styling, so the renderer can match the viewer's light or dark mode. When colour must carry meaning, choose colours that keep text and edges high-contrast in both light and dark mode.

When a lower-level actor or component performs an operation but another part of the system owns policy, authority, normalisation, interpretation, or decision-making, show those responsibilities separately in the diagram or explain the distinction immediately below.

## Maintain the working model

Reuse established terminology consistently.

When returning to an earlier concept, reconnect it to the existing model. If the discussion has moved several layers away, re-orient the user with a short recap rather than restarting.

Preserve unresolved questions when they matter to later reasoning, and revisit them when the conversation reaches the relevant part of the system.
