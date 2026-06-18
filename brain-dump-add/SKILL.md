---
name: brain-dump-add
description: Add new thoughts to a running project at any time - capture them immediately into the decisions.md Inbox so nothing is lost, then produce a full numbered impact report against PROJECT.md, CONTEXT.md, decisions.md and key documents, and apply changes only after explicit per-change approval. Use when the user has a new idea, additional thought, change of direction or says "I just thought of something", "I have another idea", "I want to add", "this changes things", "new idea for the project" on a project that already has brain-dump artifacts. Also triggers on the Czech equivalents: "napadlo mě", "mám další myšlenku", "chci doplnit", "tohle něco mění", "nový nápad k projektu".
---

# brain-dump-add

Read [references/ARTIFACTS-FORMAT.md](references/ARTIFACTS-FORMAT.md) first - it defines the artifacts, the two-phase principle, risk classes A/B and the interviewing rules.

**Interactive only.** These skills require a human in the loop (one question at a time, class A
approval per diff). Never run them inside an autonomous/non-interactive agent loop. If you detect
you are running unattended (e.g. an orchestration loop), do Phase 1 capture only if explicitly
asked, and stop before any propagation.

If no PROJECT.md or decisions.md exists, there is nothing to add to - suggest brain-dump-init instead.

This skill has two strictly separated phases. The separation exists because the two failure modes are opposite: in capture, the risk is losing a thought, so write immediately; in propagation, the risk is silently corrupting decisions the user already made, so never write without approval.

## Phase 1: Capture (writes immediately, append-only)

1. Read PROJECT.md, CONTEXT.md, decisions.md, and skim the files in Key documents.
2. Let the user dump the new thought freely, without interrupting.
3. Grill the delta only - one question at a time, recommended answer with each. Never re-open what the artifacts already answer; the whole point of the artifacts is that the user never has to explain the same thing twice. Check the user's terms against CONTEXT.md and challenge conflicts immediately.
4. As each part of the idea becomes clear, append it to decisions.md > Inbox with status `captured`, following the editing discipline in ARTIFACTS-FORMAT.md: a targeted edit that matches the `## Inbox` heading and inserts the new entry beneath it - never a whole-file rewrite - then re-read the file to confirm nothing else changed. Even if the session dies right now, the thought is safe.

When the idea is captured, tell the user and ask whether to run the impact analysis now or later. The Inbox is a valid resting state; impact can wait.

## Phase 2: Impact and propagation (no writes without approval)

5. Analyze how the captured idea relates to existing content - consistent, extending, or conflicting. For every conflict, name it precisely: "D-007 chose X because Y. This idea implies not-X. Revise the decision, or adjust the idea?" Scope is bounded: the three artifacts plus Key documents. For source code, only name the likely affected areas and recommend a separate implementation session - do not sweep the repository.

6. Output the full impact report before writing anything:

```
## Impact analysis: <idea title>

1. [A] PROJECT.md > Focus - narrow the focus to ...
   Proposed change: <concrete diff>
   Reason: ...
2. [B] CONTEXT.md - new term "..."
   Proposed: <entry>
3. [A] D-007 revision - ...
   ...
```

Every item: risk class, location, concrete proposed change (diff form), reason.

7. Approval, per the risk classes and the "What counts as approval" rules in ARTIFACTS-FORMAT.md: class A one by one (approve / modify / reject), class B as a batch, shortcuts always available. The original "work it in" request and any advance "I agree with your recommendations" apply to interview answers only - they are never approval for class A changes, because the user has not yet seen the diffs. After presenting the report, stop and wait. If the user cannot respond, the report is the final output of this turn - append it to the Inbox entry so the analysis is not lost. Additionally, if `docs/pending-decisions.md` exists (orchestrated project), append a one-line pointer there so the harness/cockpit surfaces that a human decision is waiting.

8. Apply only what was approved, following the editing discipline in ARTIFACTS-FORMAT.md: targeted find-and-replace edits only, never a whole-file rewrite. Revise a decision by replacing just its Status line; the new direction becomes a separate new decision entry. Rejected proposals go into the relevant decision's "Options considered" as rejected, with the reason. Mark the Inbox entry `propagated` (or `impact-reviewed` if only partially applied).

If any applied class A change touched PROJECT.md focus/goals or a decision, and the project has a planning/execution layer, end with a PLAN-IMPACT NOTE: tell the user which plan artifacts (development plan / sprint / STATE.md / ledger) now need to be re-derived, and recommend running the project's plan-resync step (e.g. `/resync-plan`). Do not edit those plan files yourself - that is a separate implementation/orchestration session.

After every edit, re-read the file and confirm no pre-existing entry was lost and the file is not truncated before reporting success.

