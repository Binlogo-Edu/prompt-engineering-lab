# Learning Log

## 2026-03-31
### What I studied
- Introduction: what prompt engineering is and why it matters
- The guide's experiment setup: `gpt-3.5-turbo`, `temperature=1`, `top_p=1`
- Why prompt results vary across models and settings

### Key insights
- Prompt engineering is not just writing better prompts; it is the systematic design of inputs for LLM tasks
- Prompt quality should be evaluated with an experimental mindset because outputs change with model and sampling settings
- `temperature` controls randomness; `top_p` controls how much of the probability mass is considered during token sampling

### Confusions / open questions
- Why this guide uses `temperature=1` and `top_p=1` as defaults
- When to change `temperature` versus when to change `top_p`

### Tomorrow's plan
- Continue to LLM Settings after finishing the Introduction practice notebook

## 2026-04-01
### What I studied
- Reviewed Introduction through self-test and explanation in my own words
- Clarified the difference between `temperature` and `top_p`
- Confirmed when to lower `temperature` for stable structured outputs
- Started the LLM Settings chapter warm-up

### Key insights
- Prompt engineering is about designing inputs for real tasks, not only polishing wording
- Lower `temperature` is better for predictable outputs such as JSON
- `top_p` limits token choice to a probability-mass subset, while `temperature` reshapes randomness within sampling
- In production, changing multiple decoding knobs at once makes behavior harder to predict and debug

### Confusions / open questions
- Build stronger intuition for `top_p` through examples instead of definitions alone
- Learn how model choice, max tokens, and stop sequences affect output quality

### Tomorrow's plan
- Start the LLM Settings chapter and connect parameter concepts to actual prompting behavior
