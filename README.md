# Auto Research Engineer

A Claude Code setup that implements ML papers from arxiv. Give it a paper, it orchestrates a team of AI agents to read the paper, plan the implementation, write the code, verify correctness, optimize performance, train, and compare results against the paper's claims.

## Usage

```
tmux new-session claude --dangerously-skip-permissions --settings-sources project "implement 2307.08691"
```

Or paste an arxiv URL or describe a paper. Claude handles the rest. tmux is required for agent teams to work.

## Requirements

- [Claude Code](https://claude.ai/code) with agent teams enabled
- [tmux](https://github.com/tmux/tmux)
- [uv](https://docs.astral.sh/uv/) (Python package manager)
- GPU recommended for training phases

## How it works

Claude acts as team lead and spawns specialized agents for each phase:

1. **Paper exploration** — download and read the paper, write structured notes
2. **Planning** — draft an implementation plan with critique loop
3. **Implementation** — write the code (parallel tasks use git worktrees)
4. **Verification** — check code against paper equations and algorithms
5. **Performance optimization** — iterative profiling and tuning
6. **Training** — full training run with anomaly monitoring
7. **Results verification** — compare outputs against paper claims

Each phase produces notes, decisions, and git commits. See `CLAUDE.md` for the full protocol.

## Project structure

```
workspace/<paper-name>/
├── paper/          # downloaded paper (PDF, HTML, TeX)
├── notes/          # diary entries, task notes, decisions log
├── src/            # implementation code
├── PLAN.md         # implementation plan
└── pyproject.toml
```
