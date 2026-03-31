# Introduction

## Status

Completed on 2026-04-01 after self-test review.

## Core idea

Prompt engineering is the practice of designing inputs for large language models so they can perform tasks more effectively, more reliably, and with clearer control.

It is not limited to "writing prompts nicely". It includes:

- defining the task clearly
- structuring the input
- choosing constraints and examples
- testing how the model responds
- iterating based on output quality

## What is prompt engineering

My working definition:

> Prompt engineering is a set of practices around LLMs that systematically designs prompts and other inputs to better apply, guide, and unlock model capabilities.

In plain language:

- it is about using prompts on purpose, not by guesswork
- it aims for repeatable and useful results
- it helps us understand both what LLMs can do and where they fail

## Why it matters

Researchers use prompt engineering to:

- improve safety
- test reasoning ability
- study performance on tasks such as QA and arithmetic reasoning

Developers use prompt engineering to:

- build more robust AI features
- connect models with tools and workflows
- reduce ambiguous or low-quality outputs

## Notes for this guide

The guide states that many examples are tested with:

- `gpt-3.5-turbo`
- `temperature=1`
- `top_p=1`

Important takeaway:

- prompt results depend on both the model and the decoding settings
- the same prompt can behave differently across models
- we should treat prompting as experimentation, not as a one-shot trick

## Parameter notes

### `temperature`

`temperature` controls how random the next-token sampling is.

- lower values make the model more conservative and predictable
- higher values make the model more varied and more willing to choose less likely tokens

Quick intuition:

- `temperature=0` or near 0: more deterministic, better for stable formatting and direct tasks
- `temperature=1`: balanced randomness, common general-purpose default
- above 1: more diverse but also more unstable

### `top_p`

`top_p` controls nucleus sampling. The model first sorts candidate tokens by probability, then keeps only the smallest set whose total probability reaches `p`.

- lower `top_p` means sampling from a narrower set of likely tokens
- higher `top_p` means allowing a wider candidate set

Quick intuition:

- `top_p=0.1`: very narrow and conservative
- `top_p=1`: no additional truncation beyond the full distribution

Another intuition:

- `temperature` asks: how bold should sampling be?
- `top_p` asks: how wide should the candidate pool be before sampling starts?

Simple mental model:

- if the next-token candidates were ranked as the most likely words, `top_p` decides how far down the ranked list the model is allowed to look
- then `temperature` affects how strongly the model prefers the top options within that allowed pool

### Why `temperature=1` and `top_p=1`

These defaults are useful in a guide because:

- they represent a neutral baseline rather than a tightly constrained decoding setup
- they make examples closer to default playground behavior
- they avoid hiding prompt weaknesses behind overly restrictive sampling

But they are not always the best production values.

In practice:

- use lower `temperature` when you want consistency, exactness, or formatting stability
- adjust `top_p` less often; many teams keep `top_p=1` and tune `temperature` first
- avoid changing both aggressively at the same time unless you are running controlled experiments

## What to remember from Introduction

1. Prompt engineering is systematic input design for LLMs.
2. It is valuable because it improves reliability and helps reveal model limits.
3. Prompt behavior depends on model choice and decoding settings.
4. Good prompting requires experimentation and comparison, not only intuition.

## Self-test

Try to answer these without looking back:

1. Why is prompt engineering more than "writing a better sentence"?
2. What is the main difference between `temperature` and `top_p`?
3. Why should prompt work be treated experimentally?
4. When would you lower `temperature` in a real product?

## Review result

Self-test passed on 2026-04-01.

What I got right:

- prompt engineering is tied to task performance and model capability, not just phrasing
- lower `temperature` is the right move for stable JSON output
- prompting should be treated experimentally because results vary by model and settings

One correction:

- `top_p` is not just "sampling from a large range"
- more precisely, it keeps only the smallest set of high-probability tokens whose cumulative probability reaches `p`

Useful shortcut:

- lower `temperature`: make choices less random
- lower `top_p`: shrink the menu of allowed next-token choices

## Next step

Next chapter: LLM Settings.
