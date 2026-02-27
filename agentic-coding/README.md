# agentic-coding

How to make AI coding agents actually work. Experiment on [spets](https://github.com/eatnug/spets), document here.

## Questions

### 1. Dynamic planning

Fixed-stage workflows don't fit all task sizes. Can the agent dynamically adjust its pre-implementation steps based on what knowledge is needed?

- How do you define "ready to implement"?
- How does the agent figure out what it doesn't know yet?

### 2. Verification

AI generates too much code to manually verify. Deployment becomes a gamble. How do you get confidence that the code works — beyond test cases you thought of?

- Property-based testing, contracts, formal methods — what actually works in practice?
- Can verification itself be agentic?

## Related Work

Full survey: [survey.md](./survey.md)

### Dynamic Planning

| Approach | Key Idea | Relevance |
|----------|----------|-----------|
| **HTN Planning** | Decompose tasks hierarchically, check preconditions before each step | "Do I know enough for this step?" as a structural question |
| **Think-on-Process** (2024) | Generate task-specific workflows dynamically instead of fixed pipelines | Most directly tackles our Q1 |
| **SWE-Search** (ICLR 2025) | MCTS for coding agents — treat planning as tree search | Performance scales with compute, not pipeline rigidity |
| **RepoGraph** (ICLR 2025) | Codebase as line-level graph, k-hop ego-graph retrieval | "What do I need to know?" answered by graph query, not exploration |
| **CodexGraph** (NAACL 2025) | LLM agents query graph DB of code symbols | Structured context discovery |
| **Agentless** (2024) | Simple 3-phase pipeline, 32% on SWE-bench at $0.70 | Simplicity baseline — dynamic planning must beat this |
| **ExpeL** (AAAI 2024) | Agents learn from past successes/failures without fine-tuning | Experience as planning input |

### Verification

| Approach | Key Idea | Relevance |
|----------|----------|-----------|
| **Property-Based Testing (PBT)** | Test invariants, not cases. Hypothesis, QuickCheck. | Breaks LLM test self-deception — properties are harder to fool |
| **PGS** (2025) | Use PBT to validate program properties, 23-37% improvement over TDD | Properties simpler to define than exhaustive test suites |
| **Agentic PBT** (2025) | Claude + Hypothesis in isolated environments | Verification can be agentic |
| **Vericoding** (2025) | LLMs generate formally verified code — 82% success in Dafny | Formal verification becoming practical with LLMs |
| **Design by Contract** (GPCE 2024) | Auto-generate pre/postconditions | Contracts as both spec and verification |

## The Gap

No existing system combines: knowledge graph → precondition check → dynamic plan → verification feedback loop. The pieces exist separately.

## Experiments

(tbd)
