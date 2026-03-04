# micro-gpt

Can small, local LLMs replace big expensive models for specific tasks?

## Question

Everyone depends on massive, expensive API models. But do you always need them? This project starts from the ground up — understand how LLMs work by building one from scratch, then test whether small open-source models can handle real tasks locally. No API costs, no latency, no data leaving your machine.

## Why

Two motivations:
1. **Understanding** — most people use LLMs without knowing what's inside. Build it from scratch to understand every layer.
2. **Independence** — find out where small local models are "good enough" and where they're not. Reduce dependence on big, expensive APIs.

## The Plan

### Phase 1: Reproduce (microgpt)
Based on Karpathy's [microgpt](https://karpathy.github.io/2026/02/12/microgpt/) — 200 lines of pure Python, zero dependencies.
- Read and reimplement microgpt line by line
- Understand the scalar autograd engine (`Value` class, computation graph, backprop)
- Train on the names dataset — reproduce the same generation quality
- Trace every forward/backward pass manually

### Phase 2: Dissect
- Visualize attention patterns — what is the model actually looking at?
- Ablation studies: remove components (residual connections, multi-head → single-head), measure impact
- Profile training dynamics: loss curves, gradient norms, learning rate sensitivity
- Compare scalar autograd vs PyTorch tensor ops — where does the performance gap come from?

### Phase 3: Local LLM for Real Tasks
Phase 1-2에서 쌓은 이해를 바탕으로, 작은 오픈소스 모델을 로컬에서 실용적으로 활용하는 실험.

**Core question:** 작은 모델 + fine-tuning으로 특정 태스크를 실용 수준까지 끌어올릴 수 있는가?

- 모델 후보: SmolLM2 (135M/360M), Qwen2.5 (0.5B/1.5B), Gemma 2 (2B)
- 태스크 후보: 정규식 생성, cron 표현식 생성, 커밋 메시지 생성, 음역 등
- 측정: 정확도, 추론 속도, 메모리 사용량, API 모델 대비 품질

태스크 선정은 Phase 2 이후 확정.

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
