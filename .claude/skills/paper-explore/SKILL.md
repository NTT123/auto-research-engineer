---
name: paper-explore
description: Download and deeply explore an arxiv paper as the first step of an ML paper implementation. Use this skill whenever the user provides an arxiv URL or ID and wants to implement, reproduce, or understand a paper — even if they just say "let's work on X paper", "implement this arxiv link", "read this paper", or "what does this paper do". Trigger proactively at the start of any paper implementation session.
---

# Paper Explore

The goal is to get the paper onto disk and build a thorough understanding of it before writing any code. This skill uses three agents — download, explore (researcher), and critique — with the researcher and critic communicating directly.

## Workflow

**Step 1 — Download**

Spawn the `paper-download` agent (subagent_type: `paper-download`). Pass it the arxiv ID or URL and the project directory path.

Wait for the download agent to complete before proceeding.

**Step 2 — Explore + Critique (direct loop)**

Spawn both agents, giving each the other's name so they can communicate directly:

1. **Researcher** (subagent_type: `paper-explore`, name: `researcher`): This is a **persistent teammate** — it stays alive for the rest of the project to answer paper questions from other teammates. Pass it the project directory path and the paths to the downloaded files in `./paper/`. Instruct it to:
   - Read the paper and write notes to `./notes/note-01-paper-exploration.md`
   - When done, send `SendMessage(to: "researcher-critic", message: {type: "ready_for_review"})` with a summary of what was produced
   - After the critique loop, remain alive to answer paper questions from other teammates

2. **Researcher-critic** (subagent_type: `general-purpose`, name: `researcher-critic`): This is a **temporary teammate** — shut down after the critique loop. Pass it the project directory path. Instruct it to:
   - Wait for the researcher's `ready_for_review` signal
   - Read the paper (`./paper/`) and the researcher's notes (`./notes/note-01-paper-exploration.md`)
   - Check that notes faithfully reflect the paper — flag missing details, inaccuracies, and missing source references
   - Run the direct critique loop with the researcher (max 2 iterations):
     - Send `SendMessage(to: "researcher", message: {type: "revision_needed", feedback: "..."})` with specific feedback, or `{type: "accepted"}` if no changes needed
     - Wait for researcher's `{type: "revision_done"}` if revisions were requested
   - After acceptance, send `SendMessage(to: "team-lead", message: {type: "loop_complete"})` to signal completion

Both agents can be spawned in parallel — the critic will wait for the researcher's signal before starting review.

**Step 3 — Review**

After receiving `loop_complete`, review the final `note-01-paper-exploration.md`, update task note frontmatter, and **shut down researcher-critic only**. The researcher stays alive as a persistent teammate for the rest of the project.
