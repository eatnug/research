# micro-gpt

GPT from scratch. Not a tutorial follow-along — build it, break it, understand every matrix multiply.

## Why

LLMs are the foundation of everything else in this repo (agentic-coding runs on them, multimodal is the next step). Can't push the frontier without understanding the internals: how attention decides what to focus on, why layer norm matters, what the loss landscape actually looks like.

## The Plan

### Phase 1: Reproduce
- Implement GPT-2 from scratch following nanoGPT
- Train on a small corpus (Shakespeare / Korean text)
- Match nanoGPT's results — understand every line

### Phase 2: Dissect
- Visualize attention patterns — what is the model actually looking at?
- Ablation studies: remove components, measure impact
- Profile training dynamics: loss curves, gradient norms, learning rate sensitivity

### Phase 3: Extend
- Add features not in nanoGPT: rotary embeddings (RoPE), RMS norm, GQA
- Explore: what's the minimal architecture that still "works"?
- Multimodal: can we add a vision encoder? (stretch goal)

## Math Prereqs

Documented separately: [math-for-microgpt.md](../../life/lib/math-for-microgpt.md) — covers vectors through attention, no proofs, just "why does this work in GPT."

## References

- [karpathy/nanoGPT](https://github.com/karpathy/nanoGPT) — primary reference implementation
- [karpathy/minGPT](https://github.com/karpathy/minGPT) — cleaner, more educational
- [Attention Is All You Need](https://arxiv.org/abs/1706.03762) — the paper
- [The Illustrated Transformer](https://jalammar.github.io/illustrated-transformer/) — visual walkthrough

## Experiments

(tbd)
