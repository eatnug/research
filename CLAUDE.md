# Research Repo - Claude Guide

Personal research hub. Go deep, build understanding, document findings.

## What This Repo Is

Three independent research tracks, each exploring a different axis of AI/ML:

- **micro-gpt** — Foundational. Understand how LLMs work by building one from scratch.
- **agentic-coding** — Applied AI frontier. Make AI coding agents actually work. Experiments live in [spets](https://github.com/eatnug/spets).
- **football-video-stats** — Computer vision. Extract player stats from match footage.

Things I'm curious about. Learn deep, build something, see what happens.

## Repo Structure

Each project lives on its own branch with a dedicated git worktree:

```
research/                          # main branch — repo overview
research.worktree/agentic-coding/  # agentic-coding branch
research.worktree/football-video-stats/  # football-video-stats branch
research.worktree/micro-gpt/       # micro-gpt branch
```

Projects evolve independently. Each worktree is a self-contained workspace.

## Project Structure Convention

Each project directory follows:

```
project-name/
├── README.md       # Question, survey summary, status
├── survey.md       # Literature review / related work (when applicable)
├── experiments/    # Code, notebooks, results
└── findings/       # Write-ups, conclusions
```

## Workflow

1. Start with a question worth answering
2. Survey existing work — what's solved, what's not
3. Identify the gap
4. Run experiments
5. Document findings

## Conventions

- Write in English
- READMEs: question-first, then survey, then experiments
- Commit messages: `type: description` (docs, feat, fix, refactor, experiment)
- Keep survey.md as living documents — update as new papers/tools emerge
