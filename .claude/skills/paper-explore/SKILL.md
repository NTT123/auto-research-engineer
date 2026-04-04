---
name: paper-explore
description: Download and deeply explore an arxiv paper as the first step of an ML paper implementation. Use this skill whenever the user provides an arxiv URL or ID and wants to implement, reproduce, or understand a paper — even if they just say "let's work on X paper", "implement this arxiv link", "read this paper", or "what does this paper do". Trigger proactively at the start of any paper implementation session.
---

# Paper Explore

The goal is to get the paper onto disk and build a thorough understanding of it before writing any code. This skill uses two agents in sequence — download first, then explore.

## Workflow

**Step 1 — Download**

Spawn the `paper-download` agent (subagent_type: `paper-download`). Pass it the arxiv ID or URL and the project directory path.

Wait for the download agent to complete before proceeding.

**Step 2 — Explore**

Spawn the `paper-explore` agent (subagent_type: `paper-explore`). Pass it the project directory path and the paths to the downloaded files in `./paper/`.
