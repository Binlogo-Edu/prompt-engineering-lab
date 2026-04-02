# Learning Log

This log is organized by learning progress instead of assumed calendar days.

Why this format:

- one real-world day may include multiple chapters
- a chapter may span multiple sessions
- using fixed dates per chapter created inaccurate future-dated entries

Logging rule going forward:

- record by `Session` or `Chapter` milestone
- only use an explicit date when it is certain from the current session
- prefer `Last updated` over inventing a new study date

Last updated: 2026-04-02

## Session 01

### Focus

- Introduction

### What I studied

- what prompt engineering is and why it matters
- the guide's experiment setup: `gpt-3.5-turbo`, `temperature=1`, `top_p=1`
- why prompt results vary across models and settings

### Key insights

- prompt engineering is not just writing better prompts; it is the systematic design of inputs for LLM tasks
- prompt quality should be evaluated with an experimental mindset because outputs change with model and sampling settings
- `temperature` controls randomness; `top_p` controls how much of the probability mass is considered during token sampling

### Confusions / open questions

- why this guide uses `temperature=1` and `top_p=1` as defaults
- when to change `temperature` versus when to change `top_p`

## Session 02

### Focus

- Introduction review
- LLM Settings warm-up

### What I studied

- reviewed Introduction through self-test and explanation in my own words
- clarified the difference between `temperature` and `top_p`
- confirmed when to lower `temperature` for stable structured outputs
- started the LLM Settings chapter warm-up

### Key insights

- prompt engineering is about designing inputs for real tasks, not only polishing wording
- lower `temperature` is better for predictable outputs such as JSON
- `top_p` limits token choice to a probability-mass subset, while `temperature` reshapes randomness within sampling
- in production, changing multiple decoding knobs at once makes behavior harder to predict and debug

### Confusions / open questions

- build stronger intuition for `top_p` through examples instead of definitions alone
- learn how model choice, max tokens, and stop sequences affect output quality

## Session 03

### Focus

- Prompt Elements

### What I studied

- broke prompts into instruction, context, input data, and output indicator
- reviewed a sentiment classification prompt and identified which element each line belongs to
- passed two rounds of self-test on prompt element identification and failure diagnosis

### Key insights

- not every prompt needs all four elements
- the role of each element is different: task definition, steering information, task payload, and output constraint
- output indicators are small but powerful because they shape answer format and task completion
- a translation prompt can work with only instruction, input data, and output indicator when the task is simple and unambiguous
- audience framing like `for an engineering manager` usually acts as context because it steers style and relevance rather than defining the base task

### Confusions / open questions

- when should context be included versus omitted for simpler tasks?
- how much output formatting guidance is enough before it becomes over-specified?

## Session 04

### Focus

- General Tips for Designing Prompts

### What I studied

- reviewed five design principles: start simple, write strong instructions, be specific, avoid imprecise wording, and prefer telling the model what to do
- compared weak and improved prompt examples
- passed one self-test on prompt design principles and one rewrite-focused review round

### Key insights

- prompt design is iterative, so a simple starting point is usually better than an overloaded first draft
- specificity improves reliability, but only when the details are relevant to the task
- direct instructions work better than vague style constraints
- negative-only instructions often fail; positive directions are easier for the model to follow
- large compound tasks should usually be split into smaller prompts so each step stays clear and testable
- vague phrases like `briefly`, `not too technical`, and `not too long` should be replaced by concrete constraints

### Confusions / open questions

- how specific is too specific before the prompt becomes noisy?
- when should a task be split into subtasks instead of adding more detail to one prompt?

## Next Session Plan

- begin Zero-Shot
- connect zero-shot examples back to the reusable prompt patterns in `prompts/examples-of-prompts.md`

## Session 05

### Focus

- Zero-Shot Prompting

### What I studied

- learned that zero-shot prompting means asking the model to do a task directly without examples
- reviewed the sentiment classification example as a zero-shot task
- connected zero-shot capability to instruction tuning and instruction-following behavior
- confirmed the distinction between zero-shot and few-shot through self-check and terminology review

### Key insights

- zero-shot works when the model already understands the task pattern from pretraining and instruction tuning
- zero-shot prompts rely heavily on clear instructions because there are no demonstrations to steer formatting
- if zero-shot performance is weak, the next step is usually few-shot prompting rather than adding random wording
- adding output constraints like JSON format does not break zero-shot as long as no examples are provided

### Confusions / open questions

- how to build stronger intuition for the meaning and naming of zero-shot versus few-shot

### Result

- zero-shot pass
- ready to move on to few-shot
