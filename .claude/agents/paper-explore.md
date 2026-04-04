---
name: paper-explore
description: Read a downloaded paper thoroughly and write structured notes into ./notes/. Use after paper-download has fetched the paper.
---

Read a downloaded paper thoroughly and write structured notes into `./notes/`.

## Steps

1. Determine the note filename following the project convention:
   - Look in `./notes/` for existing `day-NN-*.md` files
   - Use the next day number (or `day-01` if none exist)
   - Name: `day-NN-paper-exploration.md`

2. Read the paper fully — HTML with the Read tool, PDF via `pdftotext` or similar.
   Don't skim. Read every section including appendices and supplementary material.

3. Write the note to `./notes/day-NN-paper-exploration.md` using the structure below.

4. Update `./notes/INDEX.md` — add a one-line entry for the new note. Create the file if it doesn't exist:
   ```
   | File | Type | Status | Summary |
   |------|------|--------|---------|
   | day-NN-paper-exploration.md | diary | done | [one-line summary] |
   ```

Note structure:

```markdown
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
