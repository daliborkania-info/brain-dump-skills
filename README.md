# brain-dump - get the project out of your head, and keep it there

[Česká verze: README_cz.md](README_cz.md)

Three skills for Claude Code / Cowork (and any tool that supports the Agent Skills standard) that pull everything you know about a project out of your head, organize it into living documentation, and keep that documentation in sync with reality for as long as you work on the project. Built on the principle behind Matt Pocock's viral `grill-me` skill, but with the pieces it was missing: memory, persistence, and control over what gets written when.

## Why you want it

You know the feeling: the idea is crystal clear in your head, but the moment you start building, half the details evaporate and the other half start contradicting each other. And the AI assistant charges into the work before it understands what you actually want. brain-dump flips the roles - instead of you explaining to it, it interviews you, one question at a time, with a recommended answer attached to each (just say "yes"), and writes everything down as it goes into three files that become the project's memory. After a few sessions the AI finishes your thoughts before you do.

## What it does - three skills

**brain-dump-init** - start or adopt a project.
Either you have a fresh idea (you dump it freely, then the skill walks you through the topics), or you point brain-dump at an already-running project - it maps the repo, harvests scattered intent from the README, notes, TODO/FIXME comments and commits into draft documentation, and reviews it with you: does this match? what's missing?

**brain-dump-add** - add a thought any time, mid-project.
Something occurs to you halfway through. The skill first captures it safely (so it can't be lost), then shows you the exact impact across the whole project - what the idea changes, what it conflicts with - and only after your approval does it propagate. Risky decisions are approved one by one, trivia in a batch.

**brain-dump-sync** - reconcile the docs with reality.
Over time, code and documentation drift apart. The skill finds the contradictions (including between documents themselves), shows them to you with a proposed fix, but never decides for you which side is correct - only you know that.

## What you get - three files in the project

`PROJECT.md` (vision, focus, goals, open questions), `CONTEXT.md` (a glossary of terms - compatible with grill-with-docs) and `decisions.md` (decisions including the rejected options and the reasons). A week later, "why didn't we do this" is answered by the log, not by your memory.

## How to use it

The skills follow the [Agent Skills open standard](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/overview): a folder with a `SKILL.md` plus a `references/` file. That makes them portable across several tools.

### Claude Code (recommended)

1. Drop the three skill folders into your skills directory:
   - personal (all projects): `~/.claude/skills/`
   - project-only: `.claude/skills/` in the repo root
   So you end up with e.g. `~/.claude/skills/brain-dump-init/SKILL.md`.
2. Restart Claude Code (or start a new session) so it picks them up. Check with `/skills`.
3. Use them by intent - no need to type the skill name:
   - New project: `I have an idea for a project, let's brain-dump it.`
   - Existing repo: `Map this project and set up brain-dump on it.`
   - Mid-project thought: `I just thought of something - we should let users export to CSV. Work it into the project.`
   - Drifted docs: `Reconcile the docs with the current state of the project.`

### Claude.ai / Claude Desktop (Cowork)

1. Zip each skill folder (the `.skill` files in this folder are already zipped and ready).
2. In the app: Settings > Customize > Skills > **+** > Create skill, and upload the file. Claude reads the `SKILL.md` and shows a summary. Toggle the skill on.
3. Make sure code execution is enabled: Settings > Capabilities (Free/Pro/Max) or Organization settings > Skills (Team/Enterprise).
4. Then just talk to it the same way as above ("I have an idea for a project, let's brain-dump it").

### Other Agent Skills-compatible tools

The Agent Skills standard has been adopted beyond Claude. The same `SKILL.md` folders work - with each tool's own install location - in, among others, **OpenAI Codex CLI**, **Cursor**, **Gemini CLI**, and **GitHub Copilot**. The mechanics differ per tool (where you place the folder, how you enable it), so check that tool's "skills" or "custom instructions" docs, but the skill content is identical and needs no changes. Example once installed in any of them:

> "Adopt brain-dump on this repo - map it and review the documentation with me."

Note: triggering keywords are written in English. The skills still run the conversation in whatever language you speak (the Language rule), so you can answer in your own language; only the initial trigger phrases are English. If you want native-language triggers, add them to the `description:` line of each `SKILL.md`.

## What makes it different

It never rewrites a decision without your permission - capturing an idea and propagating it into the project are two separate steps, and between them you always see the exact impact and approve it. It won't drown you in endless interrogation (checkpoints instead of hundreds of questions). And it doesn't touch finished work - it only proposes; you decide.

Best for solo developers and small teams where the spec is born gradually in one head. For a quick one-off task it's overkill; for a project that lives for weeks and months, it's an insurance policy against your own idea slipping through your fingers.

## Credits

Built on the interview pattern from [`grill-me`](https://github.com/mattpocock/skills) and the ubiquitous-language / CONTEXT.md idea from `grill-with-docs`, both by Matt Pocock. brain-dump adds the capture/impact/approval split, bidirectional propagation, and the adoption mode for existing projects.
