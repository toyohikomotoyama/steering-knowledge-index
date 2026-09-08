# Steering + Knowledge Index: A Three-Layer Memory Model for AI-Assisted Projects

[日本語](README.ja.md)

This repository is a bilingual use-case proposal for long-lived AI-assisted software projects.

The model separates project memory into three layers:

- `docs/` — current truth and current deliverables
- `.steering/` — live work state plus chronological experience
- `.knowledge-index/` — retrieval paths back into relevant Steering records

> **Record today’s voyage, then use that memory to steer tomorrow’s voyage.**

The proposal also describes two practical consequences:

1. past episodes can help an AI agent anticipate structurally similar risks;
2. when these records live beside the code, the repository itself becomes a reverse-queryable handover artifact.

## Documents

- [Proposal (English)](proposal.md)
- [提案書（日本語）](proposal.ja.md)

## Examples

The examples are synthetic. They preserve the structure of real working records without containing real project, customer, system, host, environment, or business details.

- [`example/en/`](example/en/) — English
- [`example/ja/`](example/ja/) — Japanese

The examples intentionally make `docs/SPEC.md` clean and current, while Steering remains more uneven and chronological. This contrast is part of the model: the final Concepts are not known when an investigation begins.
