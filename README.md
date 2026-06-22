# Agentic Coding Loop

A Qwen Code skill that runs a structured, self-correcting coding workflow: **Baseline → Plan → Search → Modify → Verify → Review → Repair → Summarize**.

The first answer is rarely the final answer — this loop forces verify-and-repair before claiming done.

## Features

- **Scope classification** — auto-classifies tasks as Tactical / Medium / Strategic before starting
- **Baseline-first execution** — measures the verify command *before* any edits, so every repair is relative to a known-good reference
- **Quantitative progress tracking** — scores each attempt (tests passing, errors remaining, coverage %) and tracks deltas
- **Solution archiving** — snapshots best-known-good state with git patches; rolls back on regressions
- **Reward hacking detection** — checks for out-of-scope edits, weakened verify commands, and suspicious diffs
- **Self-critique review** — correctness, security, performance, coverage, and minimalism rubrics after verify passes
- **Up to 3 auto-repair attempts** — diagnoses root cause from structured verify output, not symptom patching
- **Anti-pattern guards** — detects thrashing, overfitting, context drift, speculative rewrites, and endless polishing

## Installation

### Quick install (recommended)

Add this repo as a Qwen Code skill:

```bash
# In any project or your home directory:
cd ~/.qwen/skills
git clone https://github.com/Klng79/agentic-coding-loop-.git agentic-coding-loop
```

Restart Qwen Code. The skill is now available as `/agentic-coding-loop`.

### Per-project install

```bash
cd your-project/.qwen/skills
git clone https://github.com/Klng79/agentic-coding-loop-.git agentic-coding-loop
```

## Usage

```
/agentic-coding-loop
```

The skill will auto-detect task candidates from your git state, open TODOs, and recent commits, then ask you to pick a task, a verify command, and an optional time budget.

You can also pass the task directly:

```
/agentic-coding-loop add a /health endpoint
```

## How it works

```
┌─────────────────────────────────────────────┐
│  Inputs: Task · Verify command · Timeout    │
└──────────────────┬──────────────────────────┘
                   ▼
┌─────────────────────────────────────────────┐
│  Scope Gate: Tactical / Medium / Strategic  │
└──────────────────┬──────────────────────────┘
                   ▼
  ┌──────────────────────────────────────┐
  │  0. Baseline   — run verify, snapshot │
  │  1. Plan       — decompose into steps │
  │  2. Search     — find files, set bounds│
  │  3. Modify     — smallest coherent edit│
  │  4. Verify     — score & judge         │
  │  5. Review     — self-critique (on pass)│
  │  6. Repair     — diagnose & fix (on fail)│
  │     ↺ max 3 attempts                   │
  │  7. Summarize  — final report          │
  └──────────────────────────────────────┘
```

### Stopping conditions

| Status | Meaning |
|--------|---------|
| `DONE` | Verify passes, review finds no critical issues |
| `BLOCKED` | Same failure 3×, missing data, or out-of-scope repair needed |
| `OUT_OF_SCOPE` | Next repair would touch off-limits files |
| `UNSAFE` | Destructive action required or reward hacking detected |
| `TIMEOUT` | Time budget exceeded |

## Research foundations

This skill incorporates patterns from frontier autonomous coding systems:

| Pattern | Source |
|---------|--------|
| Baseline-first execution | EPOCH protocol |
| Quantitative utility functions, solution archiving | STOP (Self-Taught Optimizer) |
| Fixed time budgets, metric-based iteration | autoresearch (Karpathy) |
| Automated review, self-critique loops | AI Scientist (Sakana AI) |
| Regression avoidance, archive admission | SePO |
| Reward hacking detection, evaluation integrity | AlphaEvolve / AAR findings |
| Action space boundaries | Gödel Agent |

## Requirements

- **Qwen Code** — the skill runs as a Qwen Code slash command
- **Git** — used for snapshotting and rollback

## License

MIT
