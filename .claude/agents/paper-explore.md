---
name: paper-explore
description: Read a downloaded paper thoroughly and write structured notes into ./notes/. Use after paper-download has fetched the paper.
---

Read a downloaded paper thoroughly and write structured notes into `./notes/`.

## Steps

1. Determine the note filename following the project convention:
   - Look in `./notes/` for existing `note-NN-*.md` files
   - Use the next number (or `note-01` if none exist)
   - Name: `note-NN-paper-exploration.md`

2. Read the paper fully — prefer `.tex` first (exact notation, greppable), then `.html`, then `.pdf` as last resort. Use `pages` parameter for PDFs (max 20 pages per request).
   Don't skim. Read every section including appendices and supplementary material.

3. Write the note to `./notes/note-NN-paper-exploration.md` using the structure below. Use citation format throughout: `[description](file path)` with line numbers for `.tex` files (e.g., `[Eq. 5, lines 120-125](./paper/main.tex)`).

Note structure:

```markdown
---
title: "Paper Exploration"
type: diary
status: completed
summary: ""
---

# [Full Paper Title]

**arxiv**: {id} | **Authors**: ... | **Year**: YYYY
**Venue**: (conference/journal if stated)

## Core Contribution
What does this paper do and why does it matter? What gap in prior work does it fill?
Write this as a crisp paragraph — the kind you'd use to explain the paper to a colleague.

## Method
The actual approach. Explain every key component clearly enough that someone could implement it:
- Architecture or algorithm description
- Key equations (write them out)
- Non-obvious design decisions and the reasoning behind them
- Any tricks or implementation details mentioned in the paper

## Experiments
- **Datasets**: what data was used
- **Baselines**: what the paper compares against
- **Key results**: the main numbers (copy from the paper's tables)
- **Ablations**: which components matter and by how much

## Implementation Details
Concrete reproduction info:
- Model size / hyperparameters
- Optimizer, learning rate schedule, batch size, training steps
- Hardware used and training time
- Any details that are underspecified or left to the appendix

Flag anything that seems ambiguous or underspecified.

## Open Questions
Things that are unclear from the paper, or that need experimental verification.
Include both short-term questions (needed to start coding) and long-term ones (needed to match results).

## Reproduction Plan
Concrete steps to implement and verify this paper:
1. What to implement first (the core algorithm)
2. What to test on first (a small sanity check — toy data, tiny model)
3. What the success criterion looks like (reproduce table N / figure M)
```
