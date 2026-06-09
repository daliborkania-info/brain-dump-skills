---
name: brain-dump-init
description: Initial brain dump - extract everything about a project from the user's head and/or from an existing codebase into PROJECT.md, CONTEXT.md and decisions.md. Two modes; (1) fresh idea; free monologue followed by a relentless one-question-at-a-time interview; (2) adoption of an already-running project with no brain-dump artifacts yet; map the repo, extract scattered intent (README, docs, notes, TODO comments) into draft artifacts with source references, and review them with the user. Use when the user wants to start a new project, says "brain dump", "dump my head", "I have an idea for a project", "let's think this through from scratch", or wants to deploy brain-dump onto an existing project ("map my project", "set this up for my existing repo", "adopt brain-dump on this codebase"). Also triggers on the Czech equivalents: "vysyp mě", "vysypat z hlavy", "mám nápad na projekt", "pojďme to promyslet od začátku", "zmapuj projekt", "nasaď brain-dump na tenhle projekt".
---

# brain-dump-init

Read [references/ARTIFACTS-FORMAT.md](references/ARTIFACTS-FORMAT.md) first - it defines the three artifacts (PROJECT.md, CONTEXT.md, decisions.md) and the shared rules (one question at a time with a recommended answer, checkpoints, parking high-fidelity questions, editing discipline, end-of-session commit).

## Pick the mode

Look at the project directory first:

- **Artifacts already exist** (PROJECT.md or decisions.md present): this is not an init situation - suggest brain-dump-add (new thoughts) or brain-dump-sync (reconcile docs with reality), and only proceed if the user confirms they want to start over.
- **Directory is empty or near-empty**: Mode 1 - fresh idea.
- **Project has real content (code, docs, README) but no artifacts**: Mode 2 - adoption. This is the common case when deploying brain-dump onto a running project.

## Mode 1: Fresh idea

### 1. Free dump before any questions
Invite a free monologue: "Dump everything in your head - no structure, no filter. I'll sort it out."

Do not ask questions yet. Questions asked too early channel the user's thinking into your structure instead of theirs; the raw dump preserves their actual mental model, including the half-formed parts. Let them finish completely.

### 2. Topic map
Organize the dump into a map of topics and propose a grilling order, dependencies first (you cannot decide pricing before knowing the audience). Show the map, let the user reorder or add topics. Small scopes per topic - long unbroken grilling fills the context window and degrades your own reasoning.

### 3. Grill topic by topic
Interview relentlessly within each topic until you reach shared understanding, following the interviewing rules in ARTIFACTS-FORMAT.md: one question at a time, recommended answer with every question, challenge vague terms, invent concrete scenarios for boundaries, park high-fidelity questions into Open questions instead of discussing them.

### 4. Write as you go
Persist every resolved item immediately - not in a batch at the end. A session can die at any moment; anything not written down is lost.

- Vision, focus, goals, non-goals, audience → PROJECT.md
- Resolved terms → CONTEXT.md
- Decisions, including the options that were rejected and why → decisions.md > Decision log
- Parked questions and undecided branches → PROJECT.md > Open questions

Create each file lazily, on its first entry, using the formats in ARTIFACTS-FORMAT.md. Once a file exists, further additions use targeted edits per the editing discipline in ARTIFACTS-FORMAT.md, never a whole-file rewrite.

### 5. Checkpoint
Every ~10 questions ask: continue deeper, next topic, or wrap up? Honor the answer.

### 6. Wrap up
Summarize: what was decided, what is parked, what topics were not reached. Ask the user which existing documents (if any) belong in PROJECT.md > Key documents - this list is what future impact analysis will check. Offer a git commit describing the session.

## Mode 2: Adoption of a running project

The project already contains scattered intent - in the README, in docs, in notes files, in TODO/FIXME comments, in commit messages. The job is to harvest it into draft artifacts and then verify the harvest with the user. Inferred content is a hypothesis about what the user thinks, not the truth; the review step is what turns it into truth.

### 1. Map the project
Explore, bounded (do not read every source file end to end):

- README, docs/, any *.md notes, specs, ADRs, CHANGELOG
- Manifest files (package.json, pyproject.toml, ...) for name/description/dependencies
- TODO / FIXME / HACK / NOTE comments across the code (search, do not read whole files)
- Recent commit messages if git history exists
- Directory structure as evidence of scope and architecture

Harvest four kinds of findings: vision/goals statements, decisions already made (explicit "we chose X because", or implicit in the architecture), domain terms used consistently or INconsistently, and loose ideas/TODOs that were never resolved.

### 2. Draft artifacts with provenance
Write draft PROJECT.md, CONTEXT.md and decisions.md where every extracted entry carries a source marker, e.g. `(source: README.md, unconfirmed)` or `(source: TODO in src/api.py:42, unconfirmed)`. Creating these files fresh is safe - they are new files - but nothing inferred is final until the user confirms it. Contradictions found between sources (README says X, a note says Y) go straight into the draft as open questions, never silently resolved by you.

### 3. Review with the user
Present a compact numbered summary of everything harvested: this is what your project says about itself. Then walk through it - confirm / fix / remove - the user reacts to a finished picture instead of dictating from scratch, which is faster and surfaces wrong guesses immediately. Ask about one thing at a time, with your recommendation, starting with the contradictions and the most load-bearing items (vision, focus). Explicitly ask what is MISSING: "What is in your head that is written down nowhere in the project?" - that gap is the whole reason this skill exists. If the missing part is large, run a free dump on it (Mode 1, step 1) before grilling.

### 4. Finalize
On each confirmation, remove the `unconfirmed` marker with a tar
