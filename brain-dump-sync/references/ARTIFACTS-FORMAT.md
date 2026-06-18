# Brain-dump artifacts and shared rules

This file is shared by the brain-dump skill family (brain-dump-init, brain-dump-add, brain-dump-sync). It defines the three project artifacts and the rules that apply in every session.

## The three artifacts

All three files live at the project root. Create them lazily - only when you have something to write. Never create empty scaffolding.

### PROJECT.md - what the project is

```markdown
# <Project name>

## Vision
One or two paragraphs: what this project is and why it exists.

## Focus
The current direction and priorities. This section changes when new ideas shift the project.

## Goals
- Concrete, checkable goals.

## Non-goals
- Things explicitly out of scope, with a short reason. Non-goals prevent scope creep and are as valuable as goals.

## Open questions
- [ ] Parked questions: high-fidelity questions that need a prototype to answer, and undecided branches. Each with a note on what would resolve it.

## Key documents
- `path/to/file.md` - one line on what it contains and why it matters.
```

The "Key documents" list defines the scope of impact analysis. Only files listed here (plus the three artifacts) are checked when propagating new ideas. Keep it short and current. If the project has a planning/execution layer (e.g. a development plan, `docs/sprints/`, a live `STATE.md`, a roadmap), list those files here too - otherwise a change to focus or a decision will never reach the running plan.

### CONTEXT.md - what words mean

A glossary and nothing else. No implementation details, no specs, no scratch notes. This format is compatible with Matt Pocock's grill-with-docs, so both skill families can share the file.

```markdown
# Context

One sentence describing the project domain.

## <Term>
Definition in one or two sentences.
**Avoid:** alias1, alias2 (confusable terms and why they are wrong here)
```

### decisions.md - what was decided and what is waiting

```markdown
# Decisions

## Inbox
Freshly captured ideas waiting for impact analysis. Append-only during capture.

### <date> <short title>
The clarified idea, as agreed during grilling. Status: captured | impact-reviewed | propagated

## Decision log

### D-001: <title>
- **Date:** YYYY-MM-DD
- **Decision:** what was chosen
- **Options considered:** including rejected ones, each with the reason it was rejected
- **Depends on:** D-xxx (if any)
- **Status:** leaning | active | locked | revised YYYY-MM-DD (reason)
```

Rejected options are first-class content. Weeks later, "why didn't we do X" is answered by the log instead of re-running the whole interview.

**Status semantics.** `leaning` = a working preference, not yet committed. `active` = decided
and in effect. `locked` = decided and protected: it cannot be broken without a superseding
decision (in an orchestrated project, a `locked` decision is changed only via an ADR that is
then mirrored here). `revised YYYY-MM-DD (reason)` = superseded; the new direction is a
separate new `### D-xxx` entry, never an in-place rewrite of the old one.

## Shared rules (apply in every brain-dump session)

### Two-phase principle
Capturing thoughts is append-only and happens immediately - a session can crash at any moment and nothing must be lost. Changing existing content (any edit to something already written, in artifacts or project files) happens only after the user has seen a full impact report and explicitly approved the change. Never mix the two: capture writes never modify existing lines.

### Interviewing
- Ask one question at a time and wait for the answer.
- With every question, give your recommended answer, so the user can simply say "yes". Your recommendation is also where your own knowledge adds ideas the user did not think of.
- Anything answerable from the artifacts, the codebase, or the key documents: find it yourself instead of asking.
- Challenge vague language. If the user's term conflicts with CONTEXT.md, surface it immediately ("Your glossary defines X as ..., but you seem to mean ..."). If a term is overloaded, propose a precise canonical term and record it.
- Invent concrete scenarios to probe the boundaries of concepts and edge cases.
- State facts you have gathered, state the assumptions you are making, and question those assumptions explicitly.

### Checkpoints instead of question limits
Roughly every 10 questions, pause: "Continue deeper on this topic, move to the next topic, or wrap up?" Grilling sessions are valuable but notoriously prone to running for hours; the checkpoint keeps the user in control without an arbitrary cap.

### High-fidelity questions are parked, not grilled
Questions that need a prototype, mockup, or real usage to answer ("how should this UI feel?") cannot be resolved by talking. Park them in PROJECT.md > Open questions with a note on what would resolve them, and move on.

### Bounded impact analysis
Impact analysis covers the three artifacts plus the files listed in PROJECT.md > Key documents. Never sweep the whole repository - on large projects that fills the context window, degrades reasoning quality, and produces a false sense of completeness. For source code, name the likely affected areas and recommend a separate implementation session.

### Risk classes for approval
Every proposed change to existing content gets a class:
- **Class A (risky):** changes project focus or goals, revises an existing decision, or touches finished work/documents. Approved one by one: approve / modify / reject.
- **Class B (trivial):** new glossary term, new decision with no conflicts, wording fix. Approved as a batch ("apply all class B").
When in doubt, classify as A. Always offer shortcuts: "apply all", "apply all except N", "apply from N on".

### What counts as approval (and what does not)
Approval for a class A change is valid only when the user has seen the concrete proposed diff and confirms it in a message that comes AFTER the impact report. Specifically, these do NOT count as approval:
- The initial request itself ("work it in", "update the docs", "sync it", "incorporate this") - that is a request to run the process, not consent to its outcomes.
- Advance blanket consent ("I agree with whatever you recommend", "assume I approve your suggestions") - that applies to interview ANSWERS only, never to applying class A changes. The user cannot approve a diff they have not seen.
- Your own confident hypothesis about a contradiction - even when one side is obviously stale, the user decides which side is right. You propose, you never resolve.

If the user cannot respond (non-interactive run, end of turn), output the impact report and stop. An unapplied report is a fine resting state; a wrongly applied change is not.

### Editing discipline
Decision history is the most valuable thing these files hold and the easiest thing to destroy with a careless write. Two rules protect it.

**Rule 1 - only ever touch the bytes you are changing.** Mutate artifacts with a targeted find-and-replace edit: match a unique snippet that already exists in the file and replace just that snippet. Never regenerate or rewrite a whole artifact, and never do a whole-file overwrite of a file that already exists - a whole-file write is the single operation that can silently drop or truncate everything below the point where it goes wrong. Concretely:
- New Inbox entry: match the `## Inbox` heading line and replace it with the same heading line plus the new entry beneath it. Everything else stays untouched because you never referenced it.
- New decision / glossary term / open question: match the section heading (`## Decision log`, etc.) and insert the new entry directly under it the same way, or match the last existing entry and append after it.
- Revising a decision: match only its `- **Status:** ...` line and replace just that one line; the original Date/Decision/Options stay exactly as they were, and the new direction becomes a separate new decision entry. Never rewrite a decision's history in place.
- First write into an empty project (the file does not exist yet) is the only time a whole-file create is correct.

**Rule 2 - verify every write before reporting success.** Immediately after each edit, re-read the entire file and confirm two things: every previously existing entry is still present (count the `### D-xxx` headings, the glossary terms, the sections - the count must never drop), and the file ends with a complete line, not mid-word. The content you read at the start of the session is your reference copy; if anything is missing or the file looks truncated, restore it from that reference and redo the change as a smaller, more sharply targeted replacement. Report the change as done only once the re-read passes. This catches both a bad edit and any environment-level write glitch.

### End of session
Summarize what was resolved and what remains open. If the project uses git, offer to commit with a message covering: what changed, why, what was rejected, and what remains as TODO. The git history replaces a CHANGELOG file.

### Provenance markers (adoption)
When artifact entries are inferred from existing project files rather than stated by the user, they carry a marker: `(source: <file or location>, unconfirmed)`. The marker is removed when the user confirms the entry during review. Entries still marked `unconfirmed` are hypotheses - treat them with lower confidence.

### Interplay with an orchestration harness (optional)
When the project also runs an autonomous delivery harness, keep the layers separate:
- `decisions.md > ## Inbox` is the HUMAN strategic capture channel (brain-dump). Machines may
  append a `captured` entry here, but only a human applies class A changes.
- `docs/pending-decisions.md` (if present) is the MACHINE escalation register (Tier C waiting /
  Tier B veto) - the harness owns it. It is NOT the same thing as the Inbox.
- ADRs under `docs/adr/` are mirrored into `## Decision log` as `### D-xxx` entries by the
  orchestrator, using the same editing discipline as this file (targeted insert; revise only the
  `- **Status:**` line). brain-dump-sync should treat an ADR with no matching `D-xxx`, or a
  `D-xxx` whose status contradicts its ADR, as a contradiction to report.
