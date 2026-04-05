# Auto Research Engineer

A Claude Code setup that implements ML papers from arxiv. Give it a paper, and it orchestrates a team of persistent AI agents to read the paper, plan the implementation, write the code, verify correctness, optimize performance, train, and compare results against the paper's claims.

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

Claude acts as team lead, creating one persistent team per paper. Teammates stay alive across phases, accumulating context and communicating directly with each other.

### Agent team

| Role | Phases | Responsibilities |
|------|--------|-----------------|
| **researcher** | 1-8 | Paper expert, writes exploration notes, critiques plan and docs |
| **researcher-critic** | 1 only | Critiques researcher's notes (temporary) |
| **model-engineer** | 2-8 | Drafts architecture plan, implements model code |
| **data-engineer** | 3-8 | Data loading, preprocessing, batching |
| **training-engineer** | 3-8 | Training loop, logging, checkpointing, runs full training |
| **tester** | 4-8 | Correctness verification against paper |
| **performance-engineer** | 5-8 | Profiling and optimization |
| **eval-engineer** | 7-8 | Evaluation pipeline, results comparison |
| **documentation-engineer** | 8 | Writes project README |

### Phases

1. **Paper exploration** — researcher reads the paper thoroughly, writes structured notes; researcher-critic reviews via critique loop
2. **Planning** — model-engineer drafts `PLAN.md`; researcher critiques
3. **Implementation** — model/data/training engineers implement in parallel with dependency tracking
4. **Verification** — tester checks code against paper equations and algorithms, files bugs directly with engineers
5. **Performance optimization** — iterative profile-fix-measure cycles
6. **Training** — full training run with pre-authorized emergency actions and anomaly monitoring
7. **Results verification** — eval-engineer compares outputs against paper claims
8. **Documentation** — documentation-engineer writes README; researcher reviews

Key documents (notes, plan, README) go through a **critique loop**: author drafts, reviewer critiques, max 2 revision rounds. Each phase produces notes, decisions, and git commits.

See `CLAUDE.md` for the full protocol.

## Project structure

```
workspace/<paper-name>/
├── paper/          # downloaded paper (TeX preferred, then HTML, then PDF)
├── notes/
│   ├── decisions.md    # key decisions log
│   ├── note-NN-*.md    # diary entries
│   └── task-*.md       # work orders for teammates
├── src/            # implementation code
├── PLAN.md         # implementation plan
├── README.md       # project documentation
└── pyproject.toml
```
