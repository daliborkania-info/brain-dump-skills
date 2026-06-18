---
name: brain-dump-sync
description: Reconcile brain-dump artifacts (PROJECT.md, CONTEXT.md, decisions.md) with the actual state of the project - find contradictions between the artifacts themselves and against key documents, report them all with proposed fixes, and update only after explicit approval. Use when the user says documentation is stale, "update the docs", "reconcile the docs with the project state", "sync the docs", "are the docs still accurate", or after a stretch of work that may have drifted from the documented decisions. Also triggers on the Czech equivalents: "aktualizuj dokumentaci", "srovnej dokumenty se stavem projektu", "sjednoť dokumentaci", "odpovídá dokumentace realitě".
---

# brain-dump-sync

Read [references/ARTIFACTS-FORMAT.md](references/ARTIFACTS-FORMAT.md) first - it defines the artifacts, risk classes A/B and the approval rules.

**Interactive only.** These skills require a human in the loop (one question at a time, class A
approval per diff). Never run them inside an autonomous/non-interactive agent loop. If you detect
you are running unattended (e.g. an orchestration loop), do Phase 1 capture only if explicitly
asked, and stop before any propagation.

If no artifacts exist, there is nothing to sync - suggest brain-dump-init.

Documentation that silently drifts from reality is worse than no documentation: it answers questions wrongly with full confidence. Sync exists to catch drift early and cheaply. But sync must never "fix" things on its own - a contradiction between a document and reality has two possible resolutions (the doc is stale, or the work went off course), and only the user knows which.

## Step 1: Gather

Read the three artifacts and every file in PROJECT.md > Key documents. Check that the Key documents list itself is current - files that no longer exist, or obviously important documents missing from it, are findings too. Source code is included only if the user explicitly asks and names the scope; never sweep the whole repository.

## Step 2: Find contradictions

Compare in three directions:

- **Artifact vs artifact:** a decision in decisions.md that contradicts PROJECT.md > Focus; a term used in PROJECT.md that conflicts with its CONTEXT.md definition; an Inbox entry stuck at `captured` for a long time.
- **Artifact vs key documents:** "decisions.md D-004 says A, but spec.md describes B - which is right?"
- **Internal staleness:** open questions that have clearly been answered since; goals already achieved but still listed as open; revised decisions whose revision never propagated.

For each contradiction, form a hypothesis about which side is stale, but hold it as a question, not a conclusion.

## Step 3: Report everything, change nothing

Output a numbered report before any write:

```
## Sync report

1. [A] decisions.md D-004 says A, but spec.md describes B.
   Hypothesis: spec.md is newer, D-004 is stale.
   Proposed: revise D-004 (diff below) / fix spec.md - which holds?
2. [B] CONTEXT.md: term "..." is no longer used.
   Proposed: mark as deprecated.
```

Every item: risk class, both sides of the contradiction, hypothesis, concrete proposed fix as a diff.

The user's initial request ("update the docs", "sync the docs") authorized producing this report - it did not authorize applying any of it. Per the "What counts as approval" rules in ARTIFACTS-FORMAT.md, class A fixes need the user's answer to "which side is right?" AFTER seeing this report - your hypothesis, however confident, is not a resolution. After presenting the report, stop and wait. If the user cannot respond, the report is the final output of this turn. If `docs/pending-decisions.md` exists, append a one-line pointer there so the harness surfaces the open sync decisions.

## Step 4: Approve and apply

Per the risk classes in ARTIFACTS-FORMAT.md: class A one by one, class B as a batch, shortcuts available, nothing applied without explicit instruction given after the report. Apply changes following the editing discipline in ARTIFACTS-FORMAT.md: targeted find-and-replace edits only - replace just the Status line to revise a decision, insert under a heading to add an entry - never a whole-file rewrite, and re-read each edited file afterwards to confirm nothing pre-existing was lost or truncated. Items the user does not resolve go into PROJECT.md > Open questions so they are not silently lost.

If a sync fix changed PROJECT.md focus/goals or a decision, end with a PLAN-IMPACT NOTE recommending the project's plan-resync step (e.g. `/resync-plan`), so the change reaches the live cursor. Do not edit plan files here.
