---
at: 2026-02-27
open: true
tags: [research, ai-agents, coding-agents, planning, program-synthesis, verification]
---

# Dynamic Planning for AI Coding Agents: Research Survey

Core question: How can AI coding agents dynamically figure out what knowledge/context they need before implementing a task, and adjust their planning steps accordingly — instead of following fixed-stage workflows?

---

## 1. Automated Planning in AI (STRIPS, HTN)

### What it is

Classical automated planning is about an AI system reasoning from an initial state to a goal state through a sequence of actions. The two foundational paradigms are:

- **STRIPS** (Stanford Research Institute Problem Solver): Defines a world state as a set of logical predicates, actions as operators with preconditions and effects, and goals as desired predicate states. The planner searches for a sequence of operators that transforms the initial state into one satisfying the goal. Key idea: **means-ends analysis** — identify what's missing from the goal, find an action that achieves it, then recursively plan for that action's preconditions.

- **HTN** (Hierarchical Task Networks): More expressive than STRIPS. Instead of searching for actions that achieve goal predicates, HTN decomposes **compound tasks** into subtasks recursively until reaching primitive (executable) actions. Domain knowledge is encoded as **decomposition methods** — rules for how to break a compound task into subtasks. HTN is strictly more expressive than STRIPS (undecidable in the general case). Most industrial-strength planners are HTN-based.

### Key works

- Fikes & Nilsson, "STRIPS: A New Approach to the Application of Theorem Proving to Problem Solving" (1971) — foundational paper
- Erol, Hendler & Nau, HTN planning formalization (1990s) — established the theoretical framework
- SHOP/SHOP2 planners — practical HTN implementations
- GPT-HTN-Planner (github.com/DaemonIB/GPT-HTN-Planner) — recent project combining LLMs with HTN planning for natural language task decomposition

### Connection to the core question

The analogy is direct: implementing a coding task = achieving a goal state. HTN's hierarchical decomposition mirrors how a developer breaks "implement feature X" into "understand the API → find relevant files → modify the data model → update the handler → write tests." The critical insight is **backward reasoning from the goal**: instead of following fixed steps, an HTN planner reasons about what subtasks are needed based on the specific goal structure. For coding agents, this means: given an issue description, reason backward from "issue resolved" to determine what knowledge and actions are required.

### Practical applicability

Moderate. HTN planners need hand-authored decomposition methods (domain knowledge). The challenge for coding agents is that these methods would need to be incredibly flexible — every codebase has different structure. However, the *conceptual framework* of hierarchical decomposition with precondition checking is directly applicable. An agent could check "do I have enough context about the data model?" as a precondition before attempting to generate code.

---

## 2. Program Synthesis

### What it is

Program synthesis is the automatic generation of programs from high-level specifications. Rather than writing code, the developer provides a spec (formal logic, input-output examples, natural language, or type signatures), and the synthesizer produces a correct implementation.

Key approaches:
- **Enumerative synthesis**: Search through the space of possible programs, guided by the spec
- **Deductive synthesis**: Transform the spec into code through logical deduction
- **Inductive synthesis (Programming by Example)**: Learn the program from input-output examples
- **Neural program synthesis**: Use neural networks to generate programs from specs

### Key works

- Gulwani, "Automating String Processing in Spreadsheets using Input-Output Examples" (2011) — FlashFill, the most commercially successful program synthesis
- Solar-Lezama, "Program Sketching" (2008) — partial programs + synthesis
- **"A Benchmark for Vericoding"** (Sun et al., 2025, arXiv:2509.22908, POPL 2026) — 12,504 formal specs in Dafny/Verus/Lean, benchmarking LLM generation of formally verified code. Success rates: 82% Dafny, 44% Verus/Rust, 27% Lean
- **"LLM-Assisted Synthesis of High-Assurance C Programs"** (Mukherjee et al., 2024) — pipeline with generation and verification phases, using separation logic specs
- **ASAC Benchmark** (FSE 2024) — 136 algorithm synthesis tasks from formal specifications
- SYNT 2024 Workshop — annual workshop on synthesis, increasingly integrating LLM approaches

### Connection to the core question

Program synthesis gives us the framework for thinking about what a "complete spec" looks like. Before an agent can generate code, it needs to understand the *specification* — what the code should do, what invariants it should maintain, what interfaces it must conform to. The synthesis perspective suggests that an agent's first job is to **construct the spec** from the issue description + codebase context, then generate code that satisfies it. This naturally tells the agent what knowledge it needs: enough to fully specify the input/output behavior and constraints.

### Practical applicability

High for the conceptual framework. The vericoding results (82% in Dafny) show that when you have a formal spec, LLMs are already quite capable. The bottleneck is spec construction, not code generation — which aligns perfectly with the core question. Practically, using lightweight specs (type signatures, pre/postconditions, property descriptions) could guide agent planning.

---

## 3. AI Coding Agent Research

### What it is

Since 2024, there has been an explosion of research on LLM-based agents that autonomously resolve software engineering tasks — primarily benchmarked on SWE-bench (resolving real GitHub issues). These systems vary dramatically in architecture: from simple pipelines to complex multi-agent systems with dynamic planning.

### Key systems and papers

#### Fixed-Pipeline Approaches

- **Agentless** (Xia et al., 2024, arXiv:2407.01489) — Deliberately simple 3-phase approach: localization → repair → patch validation. No agent autonomy — the LLM doesn't decide future actions. Achieved 32% on SWE-bench lite at $0.70/issue. Key insight: **simplicity is competitive.** Challenges the assumption that complex agent architectures are necessary.

- **AutoCodeRover** (Zhang et al., 2024, ISSTA 2024, arXiv:2404.05427) — Combines LLMs with AST-level code search. Works on abstract syntax tree representation, exploiting class/method structure for fault localization. Achieved 19% on SWE-bench-lite. Key contribution: **structured code search using program representation** rather than raw text search.

#### Agent-Based Approaches

- **SWE-agent** (Yang et al., 2024, NeurIPS 2024, arXiv:2405.15793) — Introduced the concept of Agent-Computer Interface (ACI): designing the tools and feedback an agent receives to complement LLM strengths/weaknesses. Custom navigation commands, context-limited outputs, file viewer/editor. Achieved 12.5% on SWE-bench. Key insight: **the interface shapes agent behavior more than the model.**

- **OpenHands** (Wang et al., 2024, arXiv:2407.16741) — Open platform for AI software development agents. Agents interact via code writing, command line, and web browsing. Now also offers an SDK for building production agents.

- **CodeR** (Chen et al., 2024, arXiv:2406.01304) — Multi-agent system with pre-defined task graphs. Different agents have different roles (like a real team). Pre-defined plans from human experts, encoded as JSON task graphs. Achieved 28.33% on SWE-bench lite. Key insight: **expert-authored task graphs convert planning into simpler decision-making for LLMs.**

#### Dynamic Planning / Search Approaches

- **SWE-Search** (Antoniades et al., 2024, ICLR 2025, arXiv:2410.20285) — Applies Monte Carlo Tree Search (MCTS) to software agent exploration. Multi-agent framework: SWE-Agent (exploration), Value Agent (feedback), Discriminator Agent (debate). 23% relative improvement over standard agents. Key insight: **performance scales with inference-time compute through deeper search**, providing a pathway to improvement without larger models.

- **Think-on-Process (ToP)** (2024, arXiv:2409.06568) — Dynamic process generation framework. Given a software requirement, uses LLMs to create *tailored process instances* — custom workflows for each task. Moves beyond static, one-size-fits-all workflows. The generated processes act as blueprints that guide the agent system architecture.

#### Experiential Learning

- **ExpeL** (Zhao et al., AAAI 2024, arXiv:2308.10144) — Agent that learns from accumulated success/failure experiences. Three stages: (1) collect experiences, (2) extract cross-task knowledge via NL operations, (3) recall insights during evaluation. Insights are iteratively refined through add/edit/vote operations. Key insight: **agents can self-improve through experience without parameter updates.**

#### Benchmarks

- **SWE-bench** (Jimenez et al., 2024) — 2,294 real GitHub issues; the standard benchmark
- **SWE-bench Pro** (2025, arXiv:2509.16941) — 1,865 harder, enterprise-level problems requiring multi-file patches
- **SWE-EVO** (2025, arXiv:2512.18470) — Multi-step software evolution tasks spanning multiple commits

### Connection to the core question

This is the most directly relevant area. The spectrum from Agentless (fixed pipeline) to SWE-Search (MCTS-guided exploration) to Think-on-Process (dynamically generated workflows) shows the field actively grappling with the core question. Key observations:

1. **Fixed pipelines work surprisingly well** (Agentless), suggesting the planning overhead may not always be worth it
2. **Search-based approaches** (SWE-Search) let agents adaptively explore without pre-specifying the plan
3. **The missing piece is knowledge-aware planning** — none of these systems explicitly reason about "what do I need to know before I can plan?" They either follow fixed steps or explore blindly. The opportunity is to combine HTN-style precondition reasoning ("I can't generate a patch until I understand the data model") with adaptive search.

### Practical applicability

Very high. All of these systems are open-source and can be experimented with. SWE-bench provides a standardized evaluation. The Think-on-Process and SWE-Search approaches are the most relevant starting points for dynamic planning research.

---

## 4. Dependency / Impact Analysis

### What it is

Static analysis techniques that determine which parts of a codebase are affected by a change. This includes:
- **Call graph analysis**: What functions call what
- **Data flow analysis**: How data moves through the program
- **Change impact analysis**: Given a change to file/function X, what else might break
- **Dependency graph construction**: Module-level and function-level dependency tracking

### Key approaches and tools

- **Traditional static analysis tools**: Tree-sitter (parsing), LSP (Language Server Protocol), ctags — extract structural information
- **AI-enhanced dependency analysis** (2024-2025): Hybrid approaches combining static analysis with LLM reasoning. LLMs supply usage intent and naming rationale where static maps define structure
- **Augment Code's Context Engine** — analyzes dependencies across 400K+ files, language-agnostic graphs of functions, classes, and dependencies
- **Intelligent Code Analysis Agent (ICAA)** concept — synthesizes static analysis strengths with LLM capabilities for understanding how changes ripple through systems
- **Aider's RepoMap** — Uses tree-sitter to extract code definitions/references, builds a NetworkX MultiDiGraph of file relationships, ranks via PageRank with personalization, formats top-ranked definitions into token-limited context. Dynamically adjusts context size based on chat state.

### Connection to the core question

This is the "knowledge discovery" layer. Before an agent can plan, it needs to know what it's dealing with. Impact analysis answers "if I change X, what else needs to change?" — which directly informs what context the agent needs to gather. An agent solving a bug in function `foo()` needs to know: what calls `foo()`? What does `foo()` depend on? What tests cover `foo()`? Static analysis provides this programmatically, without relying on the LLM to discover it through exploration.

The Aider approach is particularly instructive: rather than giving the LLM the entire codebase, use graph algorithms (PageRank) to determine what's *most relevant* and dynamically adjust the context window. This is a practical, working implementation of "figure out what you need to know."

### Practical applicability

Very high. Tree-sitter, LSP, and graph libraries (NetworkX) are all production-ready. Aider's RepoMap is open source and battle-tested. The gap is in *using this information for planning* — current tools use it for context stuffing, not for dynamically determining planning steps.

---

## 5. Knowledge Graphs for Codebases

### What it is

Representing a codebase as a graph where nodes are code entities (functions, classes, modules, variables) and edges represent relationships (calls, imports, inherits, contains, uses). This enables structured navigation and querying of code semantics beyond what text search provides.

### Key works

- **CodexGraph** (Liu et al., 2024, NAACL 2025, arXiv:2408.03910) — Integrates LLM agents with graph database interfaces extracted from code repositories. Nodes = symbols (MODULE, CLASS, FUNCTION), edges = relationships (CONTAINS, INHERITS, USES). Agent constructs and executes graph queries for context retrieval. Evaluated on CrossCodeEval, SWE-bench, and EvoCodeBench.

- **RepoGraph** (2024, ICLR 2025, arXiv:2410.14684) — Plug-in module for repository-level code structure. Line-level graph representation. Uses k-hop ego-graph retrieval: extracts immediate relationships around a search term, capturing dependencies and interactions. **32.8% average relative improvement** in resolve rate on SWE-bench-lite. Boosts both RAG (+99.63%) and Agentless (+8.56%).

- **GraphCoder** (ASE 2024) — Coarse-to-fine retrieval based on code context graphs for repository-level code completion.

- **Knowledge Graph Based Repository-Level Code Generation** (2025, arXiv:2505.14394) — Nodes represent code elements, edges depict interactions with detailed information about functionality and behavior.

- **Code Knowledge Graph for Fuzz Driver Generation** (2024, arXiv:2411.11532) — Applies code KGs to security testing.

### Connection to the core question

Knowledge graphs provide the *queryable infrastructure* that enables dynamic planning. Instead of an agent blindly searching files, it can query: "What classes inherit from BaseHandler?" "What modules import the payments module?" "Show me the 2-hop neighborhood of the function mentioned in this bug report." This transforms context discovery from exploration (slow, unreliable) to querying (fast, complete).

RepoGraph's ego-graph retrieval is particularly relevant: starting from the entities mentioned in an issue, expand outward through the graph to discover relevant context. The number of hops can be adjusted dynamically — start with 1-hop context, and if the agent determines it needs more, expand to 2 or 3 hops.

### Practical applicability

High. CodexGraph and RepoGraph are both open-source. Graph databases (Neo4j, etc.) are mature. Tree-sitter provides the parsing layer. The main challenge is keeping the graph up-to-date as code changes and determining the right granularity (too fine = noise, too coarse = misses important details).

---

## 6. Verification of AI-Generated Code

### What it is

Methods for ensuring AI-generated code is correct, going beyond standard unit testing to provide stronger guarantees.

### Key approaches and works

#### Property-Based Testing (PBT)

Instead of testing specific input-output pairs, PBT defines *properties* (invariants) that should hold for all inputs, then generates random inputs to try to falsify them.

- **QuickCheck** (Claessen & Hughes, 2000) — Original PBT library for Haskell. Foundational work.
- **Hypothesis** (MacIver, 2019) — Python PBT library, widely used.
- **"From Prompts to Properties"** (FSE 2025, ACM) — Applies PBT to evaluate LLM-generated code (StarCoder, CodeLlama) on MBPP and HumanEval. PBT exposes correctness gaps that pass@k evaluation misses.
- **Property-Generated Solver (PGS)** (2025, arXiv:2506.18315) — Framework using PBT to validate program properties instead of I/O examples. 23.1%-37.3% relative improvement over TDD methods. Key insight: **properties are simpler to define than exhaustive test oracles**, and they break the "cycle of self-deception" where LLM-generated tests share flaws with LLM-generated code.
- **Agentic Property-Based Testing** (2025, arXiv:2510.09907) — Uses Claude with Hypothesis in isolated environments to find bugs across the Python ecosystem.
- **"Can Large Language Models Write Good Property-Based Tests?"** (Vikram et al., 2023, arXiv:2307.04346) — GPT-4 with two-stage prompting successfully synthesizes valid PBT tests covering 20.5% of properties.

#### Formal Verification

- **Vericoding Benchmark** (2025, arXiv:2509.22908) — 12,504 formal specs, LLM success: 82% Dafny, 44% Verus/Rust, 27% Lean. LLM progress improved Dafny verification from 68% to 96% in one year.
- **Astrogator** (2025, arXiv:2507.13290) — Formal verifier for Ansible code. Verified correct code in 83% of cases, identified incorrect code in 92%.
- **Martin Kleppmann's prediction** (2025) — "AI will make formal verification go mainstream." LLM coding assistants are getting good at writing proof scripts, with potential for full automation.
- **VeriBench** (2025) — End-to-end formal verification benchmark.

#### Design by Contract

- **"Automated Generation of Code Contracts"** (Greiler et al., GPCE 2024) — Fine-tuned CodeT5/CodeT5+ on 14K+ Java methods with JML annotations for automatic pre/postcondition generation.
- **Contracts-over-Classes architecture** (Piovesan, 2025) — Proposes a contract layer mediating every LLM call, with semantic and type requirements on inputs/outputs plus probabilistic remediation.
- **DbC-inspired Neurosymbolic Layer** (2025, arXiv:2508.03665) — Integrates Design by Contract principles into neural agent systems for trustworthy design.

### Connection to the core question

Verification closes the loop on dynamic planning. If an agent can verify its own outputs, it can:
1. **Know when it's done** — not just "I generated code" but "I generated code that satisfies these properties"
2. **Know when it needs more context** — if verification fails, the failure mode indicates what's wrong, which informs what additional knowledge is needed
3. **Grade its own plan** — PBT properties can serve as checkpoints throughout the implementation

The PGS insight is particularly powerful: properties are easier to generate than complete test suites, and they're harder to fool. An agent could generate properties first (as a form of spec), then use them to guide and verify implementation.

### Practical applicability

Very high for PBT — Hypothesis is production-ready, and LLMs can already generate reasonable PBT tests. Moderate for formal verification — Dafny results are promising but require the codebase to use verification-friendly languages. Design by Contract is immediately applicable in languages with assertion support (Python, Java, C#).

---

## Synthesis: How These Pieces Fit Together

The core question asks for **dynamic, knowledge-aware planning** in coding agents. Here's how these six areas combine into a coherent approach:

### Phase 0: Build the infrastructure
- **Knowledge Graph** (Section 5): Index the codebase as a queryable graph (RepoGraph, CodexGraph)
- **Impact Analysis** (Section 4): Use static analysis (tree-sitter, LSP) to pre-compute dependency information

### Phase 1: Understand the goal (spec construction)
- **Program Synthesis** (Section 2): Frame the task as "construct a specification from the issue description + codebase context"
- **HTN Planning** (Section 1): Decompose "resolve this issue" into subtasks, with preconditions that check whether enough context has been gathered

### Phase 2: Dynamic context gathering
- **Knowledge Graph queries**: Starting from entities mentioned in the issue, traverse the graph to discover relevant context
- **Impact Analysis**: Determine what parts of the codebase would be affected by the change
- **Precondition checking** (HTN-style): Before each planning step, verify that the agent has sufficient information. If not, add a context-gathering subtask.

### Phase 3: Adaptive implementation
- **Search-based exploration** (SWE-Search): Use MCTS or similar to explore implementation strategies
- **Dynamic workflow generation** (Think-on-Process): Generate a task-specific workflow rather than following a fixed pipeline
- **Experience recall** (ExpeL): Leverage past successes/failures for similar tasks

### Phase 4: Verification-driven iteration
- **PBT** (Section 6): Generate properties first, use them to verify implementation
- **Formal specs** where applicable: Use pre/postconditions as both planning guides and verification criteria
- **Feedback loop**: Verification failures inform what additional context or planning is needed

### What's missing / Open research questions

1. **Precondition reasoning for knowledge**: No current system explicitly checks "do I know enough to attempt this step?" before executing. HTN provides the framework, but it hasn't been applied to coding agent knowledge management.

2. **Adaptive granularity**: When should the agent zoom in (detailed code-level analysis) vs. zoom out (architectural understanding)? Current systems use fixed granularity.

3. **Specification inference**: How to automatically construct a sufficient spec from an issue description + codebase, without requiring formal specification languages?

4. **Cost-aware planning**: Dynamic planning has overhead. When is it worth it vs. just following a simple pipeline (the Agentless lesson)?

5. **Verification-guided planning**: Using verification results (not just pass/fail, but *why* something failed) to dynamically adjust the plan.
