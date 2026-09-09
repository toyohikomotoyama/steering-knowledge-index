# Use Case Proposal: Long-Term AI Project Memory with Docs, Steering, and a Knowledge Index

## Introduction

**Steering means steering the course: record today’s voyage, then use that memory to steer tomorrow’s voyage.**

When AI coding agents are used for long-running investigation and development work, storing only “what is currently true” is often not enough for the next task.

What also matters is experience:

- where the investigation started,
- what was suspected,
- what was checked,
- which hypotheses were wrong,
- what was initially overlooked,
- what new issues were discovered along the way,
- which evidence changed the understanding,
- what became dependent on an external answer,
- and where similar work became stuck in the past.

This use case separates those concerns into three layers: `docs/`, `.steering/`, and `.knowledge-index/`.

Findings, verification, and decisions captured in Steering can cause the Current Truth in `docs/` to be created or corrected. Retrieval runs in the opposite direction: start with Current Truth, and descend through the Knowledge Index into Steering only when the question requires history, rationale, or analogous experience.

> **Current Truth first. Experience when needed.**

---

## 1. Docs — Current Deliverables

`docs/` contains the currently valid specification, verified behavior, known constraints, and other present-state deliverables.

The question this layer answers is:

> **What is true now?**

A reader should not need to work through obsolete hypotheses or investigation detours just to understand the current specification.

When an investigation changes the understanding of the system, findings captured in Steering are verified and the resulting Current Truth is reflected in `docs/`.

```text
investigation / implementation / verification
    ↓
Steering
    ↓
verified current understanding
    ↓
docs/
```

This is not deterministic replay. A human or AI interprets and verifies the experience record before updating Current Truth.

---

## 2. Steering — Current Work State and Experience Record

A Steering file is not a report written after the work is finished.

It is created when an investigation, incident response, design decision, or other task begins, when that work is worth tracing later. The AI agent updates it while doing the work.

As a result, Steering naturally retains things that may look unnecessary after the answer is known:

- initial hypotheses,
- investigation results,
- rejected hypotheses and corrections,
- questions discovered mid-investigation,
- unplanned side investigations,
- questions sent to external parties,
- waiting states,
- implementation,
- tests,
- real-device verification,
- and remaining unknowns.

These are not merely noise. They are the experience that actually occurred.

### 2.1 Do not force one file into one Concept

The unit of a Steering file is not a Knowledge item or Concept.

It may instead correspond to a date, task, investigation, incident, design decision, or change whose history is worth tracing as one experience.

For example, an investigation that starts with character encoding may uncover:

- server configuration,
- a discrepancy between documentation and the real environment,
- a reverse proxy,
- and environment-specific configuration.

At the beginning of the investigation, we do not know what the final Knowledge or Concepts will be.

The investigation scope can define what we are looking for, but it should not unnecessarily constrain what we record.

Important related findings discovered during the work remain in the same Steering record.

### 2.2 Do not rewrite history after learning the answer

Steering is updated in real time.

For example:

```text
investigate possible mojibake
    ↓
check file encodings
    ↓
discover that non-UTF-8 text is not limited to comments
    ↓
check server charset settings
    ↓
find that the settings do not explain observed production behavior
    ↓
inspect the real environment
    ↓
discover an additional server layer
    ↓
continue the investigation
```

If this is how the work unfolded, that path should remain visible.

Once the root cause is known, the record should not be rewritten to make it look as if the correct answer was obvious from the start.

Incorrect hypotheses are not erased. They are corrected by later evidence.

A dead end already explored by one AI agent is still useful experience for a future agent.

### 2.3 Current operational state may be overwritten

Steering does not need to be entirely append-only.

Fields that describe the current work state — such as TODOs, status, remaining work, or items waiting for confirmation — may be updated in place.

For example:

```text
- [x] inspect code
- [x] confirm specification
- [x] implement
- [ ] verify on device
```

There is no need to preserve every historical snapshot of that checklist.

What should remain is the reason the state changed.

If an external answer resolved a pending item, preserve the answer and what it established.

**Do not preserve the history of the TODO itself; preserve why the TODO moved.**

In other words: the progress section is the current position and may be overwritten; the events and decisions that produced that position remain chronological.

### 2.4 During work, Steering acts as a coordination surface

With this structure, Steering also serves as a live coordination surface.

The AI agent can determine:

- what is complete,
- what remains unverified,
- what is implemented but not yet verified on a real device,
- what is waiting on an external response,
- and what can be worked on immediately.

A human can simply ask:

> What remains?

The AI reads the Steering record and reconstructs the remaining work, including tasks discovered during the investigation that were not part of the original plan.

Human-provided constraints can also be recorded in the same context:

- expected response dates,
- test-environment deployment dates,
- dates when device verification is possible,
- release dates.

Steering is therefore more than a TODO list. It is a shared surface for the human and the agent to understand the current position of the work.

### 2.5 Today’s progress record becomes tomorrow’s experience record

During active work, a Steering file contains both the current state and the path that led to it.

When the work is complete, verified Current Truth is reflected in `docs/`.

The Steering file is not discarded.

What was a progress record yesterday becomes an experience record today.

Later, when similar work appears, that experience can be retrieved again.

**Yesterday's progress record becomes tomorrow's history, and that history can become the next task's risk signal.**

This is also why the word “Steering” happens to fit the role the practice evolved into:

**record today’s voyage, then use that memory to steer tomorrow’s voyage.**

### 2.6 Use past experience to anticipate recurring risk

Suppose a previous task followed this pattern:

```text
specification is ambiguous
    ↓
confirmation is postponed
    ↓
implementation eventually requires an external decision
    ↓
work blocks waiting for an answer
    ↓
later stages become compressed
```

If that episode remains in Steering, a future AI agent encountering a structurally similar task can notice the pattern and suggest:

> Last time, this confirmation became a late-stage dependency. It may be better to ask now.

This is not deterministic prediction.

It is experience-based risk anticipation: surface structurally similar past failures early enough to reduce the chance of repeating them.

---

## 3. Knowledge Index — Retrieval Paths into Experience

As Steering grows, another problem appears:

> I remember investigating something like this before, but I do not remember which Steering file contains it.

`.knowledge-index/` exists to solve this retrieval problem.

It is not the place where Knowledge itself is stored.

An entry mainly contains:

- a topic name,
- related words,
- date,
- Steering file,
- exact heading in that Steering file.

### 3.1 Do not duplicate Current Truth and Experience

If a question can be answered entirely from the current specification, verified behavior, or configuration in `docs/`, the Knowledge Index should not duplicate that information.

The index becomes relevant when the question asks for history, rationale, or analogous experience, such as:

- Why is the current specification this way?
- Did we have a similar problem before?
- Where did the previous task get stuck?
- Which confirmation turned out to matter later?

**The Knowledge Index connects Current Truth to past experience without collapsing them into the same layer.**

### 3.2 Give one experience multiple entry points

A single Steering file may naturally contain topics such as:

- character encoding,
- server architecture,
- discrepancy between documentation and the actual environment.

Instead of assigning the Steering file to one category, the Knowledge Index provides multiple entry points into the same experience.

```text
character encoding ───┐
server architecture ──┼→ same Steering record
environment mismatch ─┘
```

This allows a future user or agent to rediscover the same experience using language that differs from the wording used when the work originally happened.

### 3.3 Do not store a fixed summary

As a rule, the Knowledge Index does not store a narrative summary of the experience.

The same historical record can be read differently depending on the current question:

- Why is the current specification this way?
- Did this bug happen before?
- Where did the previous task get stuck?
- What should we confirm earlier this time?

If we compress the experience into one fixed summary, we choose one interpretation at storage time.

That summary can also become stale as later experience or corrections accumulate.

Instead, the Knowledge Index says:

> Relevant past experience is here.

The current AI then reads the original record in the context of the current question and reconstructs the Knowledge needed now.

### 3.4 Grow bottom-up

The taxonomy is not designed up front.

Start with a catch-all file such as `misc.md`, adding topics that people or agents actually need to find later.

Split the index only when volume or natural clusters make a split useful.

Steering is historical experience and should not be casually reorganized.

The Knowledge Index is the current retrieval interface and may evolve as current retrieval needs change.

---

## 4. Relationship Between the Three Layers

There are two different flows: **updating Current Truth** and **retrieving prior experience**.

### Update flow

```text
.steering/
    Current Work State
    + Chronological Experience
        ↓
    investigation / verification / decisions
        ↓
docs/
    Current Truth
```

Experience recorded in Steering can change the current understanding, and verified findings are reflected in Docs.

### Retrieval flow

```text
current question
    ↓
docs/ — Current Truth
    ↓
Can Current Truth answer it?
    ├─ Yes → answer
    └─ No
         ↓
.knowledge-index/ — Retrieval Paths into Experience
         ↓
relevant .steering/
         ↓
read the original record in the current context
         ↓
reconstruct the Knowledge needed now
         ↓
decision / next action / warning
```

The roles are therefore:

```text
docs/
    Current Truth
    "What is true now?"

.steering/
    Current Work State
    + Chronological Experience
    "What happened, where are we now, and how did we get here?"

.knowledge-index/
    Retrieval Paths into Experience
    "Where are the relevant memories?"
```

The important distinction is that **stored experience is not the same thing as present cognition**.

The same experience record can support different Knowledge depending on the question and current situation.

---

## 5. Why This Fits AI Agents

This approach is comparatively inexpensive because the AI agent is already participating in the work.

For a human, reconstructing after the fact:

- what they thought,
- what they checked,
- where they were wrong,
- and why they changed direction

can be expensive and biased by hindsight.

The AI agent, however, is already participating in the hypotheses, searches, code reading, corrections, implementation, and tests.

It can record what is known while that context is still alive.

Steering is therefore not a report produced separately from the work.

It is **an experience record produced naturally from the work itself**.

### 5.1 The repository becomes a reverse-queryable handover artifact

Keeping Steering and the Knowledge Index in the same repository as the code provides another practical benefit:

**the repository itself becomes a reverse-queryable handover artifact for a successor or a new AI agent.**

Traditional handover documentation requires the author to predict what a future reader will need to know and organize that information in advance.

But future questions are difficult to predict.

A successor may ask:

- Why is the current specification this way?
- Is it safe to change this configuration?
- Did we have a similar bug before?
- Where did this kind of work become difficult last time?
- Why did this task require an external confirmation?
- Is there something we should confirm earlier before making the next change?

The three-layer model does not require all of those answers to be pre-edited into one handover document.

Instead, the repository contains:

```text
current code
    +
docs/                 Current Truth
    +
.knowledge-index/     entry points from current questions into the past
    +
.steering/            historical investigation, decisions, corrections,
                      confirmations, and failures
```

A successor or AI agent begins with Docs to understand the current state. Only when rationale, history, or analogous experience is needed does it use the Knowledge Index to find relevant Steering and trace backward into the experience that produced the current state.

This is fundamentally different from a handover document that must be read in the order chosen by its author.

It is a **reverse-queryable handover system**, navigated from the reader’s current question.

The records are also not created solely for handover.

Steering already exists as part of everyday investigation and implementation, and the Knowledge Index grows as a retrieval layer over those experiences.

Therefore, when ownership changes, the team does not suddenly begin “writing the handover document.” **Everyday development activity has already been continuously producing it.**

What is transferred is not only code and current specification.

It is also **the project’s accumulated experience: what happened, what was learned, and why the project is in its present state.**

---

## 6. Questions for OKF

OKF already provides `log.md` for Concept or bundle change history.

The history preserved by Steering is different in kind:

- `log.md` — **What changed?**
- Steering — **How did we get here?**

Steering preserves not only the resulting change, but hypotheses, rejected paths, dead ends, surprises, external waiting states, and corrections encountered on the way to Current Knowledge.

So the claim is not that OKF has no history mechanism. The deeper question is: **should change history and experiential history be treated as the same kind of history?**

This use case raises several questions for OKF:

1. Should Current Knowledge and the Experiential History that produced it be represented as the same unit?
2. How should OKF relate to experience records that are organized by time or work episode and may contain multiple Concepts?
3. Can multiple Retrieval Paths into original records be a useful OKF usage pattern without duplicating the Knowledge itself?
4. Is separating Current Truth / Experiential History / Retrieval Index — and reading Current Truth first — a useful convention for long-lived AI agents?
5. How should we think about a lifecycle in which a live coordination surface becomes an Experience Record after completion and later supports analogous-risk detection?
6. As Steering records accumulate, how should an agent decide when it has retrieved enough prior experience without prematurely stopping exploration or reading without bound?

This is not a demand for a specific OKF specification change.

It is a use case intended to discuss a practical question that emerges in long-running AI-assisted development:

> **What is lost when we preserve only current Knowledge?**

---

## Conceptual Note — Structural Similarity to Event Sourcing

This model emerged from practical use. Only afterward did we notice that it has a structural resemblance to Event Sourcing.

At a conceptual level:

```text
Steering        ≈ history of experience and events
Docs            ≈ Current Truth / materialized-view-like output
Knowledge Index ≈ read / retrieval index over experience
```

But this is not strict Event Sourcing.

Steering contains more than structured events: hypotheses, rejected hypotheses, conversations, external confirmations, and accidental discoveries. Docs is also not reconstructed by deterministic replay.

**The resemblance is that history contributes to current state; the difference is that humans or AI interpret and verify experience before updating Current Truth.**

---

## Conceptual Note — Structural Similarity to Yogācāra

This model was not designed from Buddhist philosophy.

After developing it through practical use, we noticed an interesting structural similarity to **Yogācāra**, particularly the distinction between accumulated traces of experience and cognition formed in the present.

This is not intended as a strict mapping between AI architecture and Buddhist concepts. Traditional Yogācāra discusses eight consciousnesses, so any analogy involving Ālayavijñāna and the six consciousnesses is necessarily selective.

In this model, Steering accumulates traces of experience.

A current question triggers retrieval through the Knowledge Index, after which the current AI rereads relevant experience in the present context and forms a current understanding or decision.

```text
accumulated past experience
        ↓
retrieval triggered by a current question
        ↓
understanding formed in the present context
        ↓
decision / action
        ↓
new experience is accumulated again
```

The point is not to turn Buddhist terminology into an AI architecture.

The useful design intuition is simpler:

> **Stored experience is not the same thing as present cognition.**

Instead of storing past experience as a fixed answer, preserve it as experience and allow meaning to be formed again from the present situation.

The similarity is therefore retrospective: a practical model happened to converge on a structure recognizable in an older conceptual tradition.
