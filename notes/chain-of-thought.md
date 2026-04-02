# Chain-of-Thought (CoT) Prompting

## Status

Completed on 2026-04-02 after concept check.

## Core idea

Chain-of-Thought prompting adds intermediate reasoning steps to the demonstrations, so the model learns to show its work before giving a final answer. This enables reliable performance on tasks that require multi-step reasoning.

## Why CoT is needed

Few-shot prompting fails on reasoning tasks because the demonstrations only show final answers. The model learns the output format but not how to derive the answer.

CoT fixes this by showing the reasoning process inside each demonstration:

```text
The odd numbers in this group add up to an even number: 4, 8, 9, 15, 12, 2, 1.
A: Adding all the odd numbers (9, 15, 1) gives 25. The answer is False.
```

Now the model can follow the same step-by-step pattern for new inputs.

## Three variants of CoT

### 1. Few-shot CoT

Provide several demonstrations that each include the full reasoning chain. The model learns to replicate the reasoning pattern.

- typically works with as few as 1 example
- more examples add robustness but 1-shot is often enough
- the reasoning steps in the demonstration are the key ingredient

### 2. Zero-shot CoT

Add the phrase **"Let's think step by step"** to the end of the prompt, with no demonstrations at all.

Introduced by Kojima et al. (2022). Simple example:

```text
I went to the market and bought 10 apples...

Let's think step by step.
```

The model generates its own reasoning chain before answering. Surprisingly effective for many reasoning tasks without any labeled examples.

Useful when:
- there are not enough examples to write demonstrations
- the task changes frequently so hand-crafting demos is impractical

### 3. Auto-CoT (Zhang et al. 2022)

Automates the process of creating few-shot CoT demonstrations to avoid manual effort and suboptimal hand-crafted examples.

Two stages:
1. **Question clustering** — partition a dataset of questions into clusters by similarity
2. **Demonstration sampling** — pick one representative question per cluster, then use Zero-Shot-CoT ("Let's think step by step") to auto-generate its reasoning chain

Simple heuristics filter out bad chains:
- question length (e.g., ≤ 60 tokens)
- number of reasoning steps (e.g., ≥ 5 steps)

This produces diverse, representative demonstrations without human labeling.

## Key properties and constraints

- CoT is an **emergent ability**: it only appears in sufficiently large language models. Small models do not benefit from CoT prompting.
- the reasoning steps must be correct and representative — bad chains in demonstrations can mislead the model
- for Auto-CoT, diversity across clusters is critical to avoid error propagation

## CoT vs few-shot summary

| | Few-shot | Few-shot CoT | Zero-shot CoT |
|---|---|---|---|
| Demonstrations | Yes | Yes | No |
| Reasoning steps shown | No | Yes | Auto-generated |
| Works on reasoning tasks | No | Yes | Often yes |
| Effort required | Low | Medium | Very low |

## My summary

- CoT adds intermediate reasoning steps so the model learns how to solve problems, not just what format to answer in
- zero-shot CoT ("Let's think step by step") is a surprisingly powerful trick that requires no examples
- Auto-CoT removes the manual work of writing demonstrations by clustering questions and generating chains automatically
- CoT only works on large enough models — it is an emergent capability

## Self-test

1. What is the key difference between few-shot and few-shot CoT?
2. What does "Let's think step by step" do and why does it work?
3. Why does CoT fail on small language models?
4. What problem does Auto-CoT solve, and how does it work?
5. When would you choose zero-shot CoT over few-shot CoT?

## Review result

Self-test passed on 2026-04-02.

What I got right:

- CoT adds intermediate reasoning steps to demonstrations; the model learns *how* to solve problems, not just the answer format
- "Let's think step by step" works as an explicit instruction AND activates reasoning patterns from pretraining
- CoT is an emergent ability that only appears in large models; small models lack the pretraining depth to replicate reasoning chains
- Auto-CoT solves the manual effort and suboptimality of hand-crafted demos by clustering questions and auto-generating chains with Zero-shot CoT
- Zero-shot CoT suits open-ended exploration and low-resource situations; Few-shot CoT suits tasks with strict format and convergent reasoning

Key insight from discussion:

- the choice between zero-shot and few-shot CoT is a trade-off between **diversity/autonomy** and **control/convergence**

## Next step

Move on to Self-Consistency prompting.
