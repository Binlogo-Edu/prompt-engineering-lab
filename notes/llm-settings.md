# LLM Settings

## Status

In progress on 2026-04-01.

## Core idea

A prompt does not determine the output by itself. Model choice and decoding settings also shape the result.

## Key settings to learn

- model: different models have different capabilities, context windows, latency, and cost
- temperature: controls randomness
- top_p: controls how wide the candidate token pool is before sampling
- max tokens: limits response length
- stop sequences: tell the model where to stop generating

## What I already understand

- lower `temperature` helps with stable structured outputs
- higher `temperature` helps with variation and creativity
- changing both `temperature` and `top_p` aggressively at the same time makes results harder to interpret

## Why settings matter

- they affect consistency
- they affect creativity
- they affect output length and truncation
- they affect reproducibility in experiments
- they affect user experience in production

## Production mindset

When tuning settings:

1. start from a clear task
2. change one important variable at a time
3. compare outputs against a simple rubric
4. keep defaults unless there is a clear reason to move them

## Warm-up check

1. For extraction or classification, why is lower `temperature` often better?
2. For brainstorming, why might higher `temperature` help?
3. Why is it risky to change multiple knobs at once?

## Next step

Continue with concrete examples of settings and their tradeoffs.
