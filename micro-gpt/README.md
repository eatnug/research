# micro-gpt

GPT from scratch. Not a tutorial follow-along — build it, break it, understand every matrix multiply.

## Why

Everyone uses LLMs but few people actually know what's happening inside. I want to understand every layer — from autograd to attention, from tokenizer to generation. Build it, break it, see what happens.

## The Plan

Based on Karpathy's [microgpt](https://karpathy.github.io/2026/02/12/microgpt/) — 200 lines of pure Python, zero dependencies. Autograd engine, tokenizer, transformer, optimizer, training loop, inference — all in one file. This is the starting point.

### Phase 1: Reproduce
- Read and reimplement microgpt line by line
- Understand the scalar autograd engine (`Value` class, computation graph, backprop)
- Train on the names dataset — reproduce the same generation quality
- Trace every forward/backward pass manually

### Phase 2: Dissect
- Visualize attention patterns — what is the model actually looking at?
- Ablation studies: remove components (residual connections, multi-head → single-head), measure impact
- Profile training dynamics: loss curves, gradient norms, learning rate sensitivity
- Compare scalar autograd vs PyTorch tensor ops — where does the performance gap come from?

### Phase 3: Extend
- Swap in a larger dataset (Shakespeare / Korean text)
- Scale up: what breaks when you increase parameters beyond microgpt's 4,192?
- Add features: rotary embeddings (RoPE), RMS norm, GQA
- Explore: what's the minimal architecture that still "works"?

## Math Prereqs

Documented separately: [math-for-microgpt.md](../../life/lib/math-for-microgpt.md) — covers vectors through attention, no proofs, just "why does this work in GPT."

## References

- [karpathy/microgpt](https://karpathy.github.io/2026/02/12/microgpt/) — primary reference. 200 lines, pure Python, zero dependencies
- [microgpt.py (gist)](https://gist.github.com/karpathy/8627fe009c40f57531cb18360106ce95) — source code
- [karpathy/nanoGPT](https://github.com/karpathy/nanoGPT) — PyTorch-based, larger-scale reference
- [Attention Is All You Need](https://arxiv.org/abs/1706.03762) — the paper
- [The Illustrated Transformer](https://jalammar.github.io/illustrated-transformer/) — visual walkthrough

## Experiments

(tbd)
