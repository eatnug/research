# llm-from-scratch

LLM의 기초부터 최신 학습 기법까지, 직접 만들고 돌려보면서 이해하기.

## Question

Transformer 기반 모델이 어떻게 작동하는지, 그리고 현대 학습 기법들이 왜 필요한지를 밑바닥부터 체감할 수 있는가?

## Why

LLM을 API로 쓰는 건 쉽지만, 안에서 뭐가 일어나는지 모르면 한계가 온다. autograd부터 시작해서 GPT를 직접 만들고, 최신 기법들을 하나씩 적용해보면서 ML 엔지니어링 기초 체력을 쌓는다.

## The Plan

### Phase 1: micrograd
Karpathy의 [micrograd](https://github.com/karpathy/micrograd) — 스칼라 autograd 엔진.
- `Value` 클래스, 연산 그래프, backpropagation 구현
- 간단한 neural network 학습
- forward/backward pass를 수동으로 추적

### Phase 2: nanoGPT
Karpathy의 [nanoGPT](https://github.com/karpathy/nanoGPT) — PyTorch 기반 GPT 구현.
- Transformer 아키텍처 이해 (attention, positional encoding, layer norm)
- Shakespeare 등 작은 데이터셋으로 학습/생성
- 학습 파이프라인 전체를 직접 구축

### Phase 3: 현대 학습 기법 적용
nanoGPT를 베이스로 최신 기법들을 하나씩 적용하고 효과를 측정.
- Mixed precision (bf16/fp16)
- Flash Attention
- Gradient accumulation / checkpointing
- AdamW, cosine LR schedule, weight decay
- LoRA / QLoRA fine-tuning
- Quantization (GPTQ, AWQ, GGUF)
- 기법별 학습 속도, 메모리, 성능 비교

## References

- [karpathy/micrograd](https://github.com/karpathy/micrograd) — scalar autograd engine
- [karpathy/nanoGPT](https://github.com/karpathy/nanoGPT) — minimal GPT in PyTorch
- [Attention Is All You Need](https://arxiv.org/abs/1706.03762) — the paper
- [The Illustrated Transformer](https://jalammar.github.io/illustrated-transformer/) — visual walkthrough

## Experiments

(tbd)
