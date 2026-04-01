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
- Begin Basics of Prompting and build intuition for instruction, context, zero-shot, and few-shot formats

## 2026-04-02
### What I studied
- Started Prompt Elements
- Broke prompts into instruction, context, input data, and output indicator
- Reviewed a sentiment classification prompt and identified which element each line belongs to
- Passed the first self-test on prompt element identification
- Passed a second round focused on prompt analysis and failure diagnosis

### Key insights
- Not every prompt needs all four elements
- The role of each element is different: task definition, steering information, task payload, and output constraint
- Output indicators are small but powerful because they shape answer format and task completion
- A translation prompt can work with only instruction, input data, and output indicator when the task is simple and unambiguous
- Audience framing like "for an engineering manager" usually acts as context because it steers style and relevance rather than defining the base task

### Confusions / open questions
- When should context be included versus omitted for simpler tasks?
- How much output formatting guidance is enough before it becomes over-specified?

### Tomorrow's plan
- Move on to General Tips
