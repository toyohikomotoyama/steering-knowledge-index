# CLAUDE.md

## 1. Steering file policy (`.steering/`)

### Reading

- Before non-trivial investigation, incident response, or design work, check whether relevant Steering files already exist.
- Existing Steering records reflect what was confirmed and concluded at that time. They may no longer be correct. Revalidate against the current code, documentation, and environment.
- If similar past work involved rework or waiting on confirmation, check whether the same condition exists now.

### Creating

- For work whose history will be worth tracing later, create `.steering/YYYY-MM-DD-short-slug.md` when the work begins.
- Do not restrict one file to one Concept. Important related findings discovered during the investigation may remain in the same Steering file even when they fall outside the original investigation scope.
- Include at least `Date`, `Related`, and `Status` near the top.

### Recording

- Update during investigation, implementation, and verification rather than reconstructing the story after completion.
- Preserve hypotheses, results, decisions, and corrections so the path remains visible. Do not rewrite an incorrect hypothesis as if it never existed.
- Current operational fields such as TODOs, status, remaining work, and waiting states may be updated in place.
- There is no need to preserve old snapshots of TODOs themselves. Preserve the answer, discovery, decision, or verification that caused their state to change.
- Add newly discovered checks or side issues as they appear.
- When the AI cannot decide something alone, record what must be confirmed and with whom.
- Record constraints that affect work sequencing, such as deadlines, expected response dates, or device-verification availability.

## 2. Knowledge Index policy (`.knowledge-index/`)

- `docs/` contains deliverables and current truth.
- `.steering/` preserves work history and experience.
- `.knowledge-index/` is an index into Steering, not a summary store for docs or Steering.
- An entry should primarily contain a topic, related words, and chronological references.
- Preserve the exact Steering heading in each reference.
- The same Steering file or heading may be referenced from multiple topics.
- Do not design the taxonomy up front. Start with `misc.md` and split later only when useful.
