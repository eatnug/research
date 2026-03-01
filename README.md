# research

Digging deep, figuring things out.

Don't just use AI — understand it deeply enough to build and improve it.

## Why

Each project targets a different axis of the AI landscape:

- **Foundational**: How do LLMs actually work, from matrices to attention?
- **Applied frontier**: AI writes code now. How do we make it reliable?
- **Different modality**: Can CV extract structured data from unstructured video?

The goal isn't papers or products. It's building real understanding — the kind you can only get by doing.

## Projects

| Project | What | Status |
|---------|------|--------|
| [micro-gpt](./micro-gpt) | GPT from scratch — understand the transformer inside out | upcoming |
| [agentic-coding](./agentic-coding) | Making AI coding agents actually work — dynamic planning + verification | ongoing |
| [football-video-stats](./football-video-stats) | Player stats from match footage via CV | upcoming |

## Structure

Each project lives on its own branch with a dedicated worktree. They evolve independently.

```
research/                          # main — this overview
research.worktree/agentic-coding/  # experiments in AI coding agents
research.worktree/football-video-stats/  # CV pipeline for football analytics
research.worktree/micro-gpt/       # building GPT from scratch
```

Each project follows: **question → survey → gap → experiments → findings**.
