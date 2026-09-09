# Steering + Knowledge Index: Giving AI Agents a Memory of Yesterday

[日本語](README.ja.md)

AI agents have become remarkably capable.

They can read code, understand specifications, investigate bugs, implement changes, and run tests.

But after working with them on a long-lived project, one problem becomes hard to ignore:

**they forget yesterday.**

We can preserve the final, correct specification in `docs/`.

What tends to disappear is the experience that produced it:

- what we suspected,
- which hypotheses failed,
- what we overlooked,
- where the work became blocked,
- which external answer we had to wait for,
- and what unexpected facts appeared along the way.

When a similar problem appears months later, the next agent often starts again from the answer rather than from the experience.

This repository proposes a three-layer project-memory model that grew out of that problem.

## Three layers

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

`docs/` holds the currently valid specification, verified behavior, and known constraints.

`.steering/` is written while work is happening. It keeps hypotheses, dead ends, corrections, waiting states, implementation, verification, and remaining work in the order they actually occurred.

`.knowledge-index/` does not copy or summarize that experience. It provides retrieval paths back into the relevant Steering records.

The read order is deliberately simple:

> **Current Truth first. Experience when needed.**

Start with `docs/`. If the current truth answers the question, stop there.

Descend through `.knowledge-index/` into `.steering/` only when the question asks for history: *Why is it this way? Did this happen before? Where did we get stuck last time?*

## Steering is not a postmortem

A Steering file is created when the investigation or change begins, not after the answer is known.

If the work unfolds like this:

```text
hypothesis A
  ↓
investigate
  ↓
wrong
  ↓
discover another setting
  ↓
real environment still does not make sense
  ↓
ask another question
  ↓
discover another architectural layer
  ↓
correct the understanding
```

then that path stays visible.

We do not rewrite the record afterward to make the correct answer look obvious from the beginning.

The wrong turn matters because a future agent may be about to take the same one.

Operational state is different. TODOs, status, and remaining work may be updated in place.

**Do not preserve every historical TODO snapshot; preserve why the TODO moved.**

That makes the same Steering file useful twice: during active work it is a coordination surface; after completion it becomes an experience record.

Yesterday's progress record becomes today's history. Today's history can become tomorrow's risk signal.

## Do not force one experience into one Concept

Investigations do not respect taxonomies.

An encoding investigation may unexpectedly reveal an undocumented proxy or a mismatch between documentation and the real environment.

If we decide up front that the record belongs to one Concept, we risk throwing away discoveries that do not fit that label.

So Steering is organized around **work episodes and investigations**, not around a predefined knowledge taxonomy.

Later, the Knowledge Index can give the same experience multiple entry points:

```text
character encoding ───┐
server architecture ──┼→ same Steering record
environment mismatch ─┘
```

The taxonomy grows bottom-up as real retrieval needs appear. Start with a catch-all such as `misc.md`; split it only when accumulated experience produces useful clusters.

## Why an index instead of a summary?

The meaning of the same past episode changes with the question being asked today.

*Why does this specification exist?* and *Are we about to repeat the same failure?* may point to the same Steering record but require different readings of it.

So the Knowledge Index does not freeze one narrative at storage time.

**Preserve the experience. Reconstruct its meaning in the present context.**

And if the reconstruction becomes questionable, return to the original record.

> **Stored experience is not the same thing as present cognition.**

## Make the repository itself a handover artifact

Traditional handover documents require their authors to predict the questions a future maintainer will ask.

That is impossible to do completely.

With the three-layer model, a successor or a new AI agent can travel backward from a question asked today:

```text
current question
  ↓
read current truth in docs/
  ↓
history / rationale / analogous experience needed
  ↓
.knowledge-index/
  ↓
relevant .steering/
  ↓
reread the original experience in today's context
```

Everyday development therefore builds a **reverse-queryable handover artifact** without waiting for a handover event.

## Familiar shapes, discovered afterward

After this practice emerged from day-to-day engineering, we noticed structural similarities to several older ideas.

It resembles Event Sourcing in that Steering preserves a history while Docs represents something closer to current state. But this is not deterministic event replay: Steering contains hypotheses, mistakes, conversations, and external confirmations that humans or agents interpret and verify before updating Current Truth.

There is also a loose structural resemblance to Yogācāra, the Buddhist tradition that distinguishes accumulated traces of experience from cognition arising in the present. This was not a design source; the resemblance became visible only after the practical model existed.

Even the image in *Hōjōki* — the flowing river that continues while its water is never the same — feels unexpectedly close to a model in which the current state changes while traces of its history remain available.

These are analogies, not technical equivalences.

The point is that **a model built to solve a practical memory problem happened to converge on structures recognizable elsewhere.**

## Why “Steering”?

The name was not chosen to support this philosophy. It was simply a term already in use by another engineer.

But as the practice evolved, the name started to fit unusually well.

> **Record today’s voyage, then use that memory to steer tomorrow’s voyage.**

What do we lose when we preserve only current Knowledge?

Does a long-lived AI agent need Knowledge alone — or does it also need the ability to **remember experience**?

## Details

- [Proposal (English)](proposal.md) — model, operating rules, and questions for OKF
- [提案書（日本語）](proposal.ja.md)
- [`example/en/`](example/en/) — synthetic English example
- [`example/ja/`](example/ja/) — 架空の日本語サンプル

The examples are synthetic. They preserve the structure of real working records without containing real project, customer, system, host, environment, or business details.
