# Zero-Shot Prompting

## Status

Completed on 2026-04-02 after concept check.

## Core idea

Zero-shot prompting means asking the model to perform a task directly without giving any examples or demonstrations in the prompt.

## Definition

A zero-shot prompt:

- gives the model an instruction
- may include the task input
- does not include labeled examples showing how to do the task

Simple example:

```text
Classify the text into neutral, negative or positive.

Text: I think the vacation is okay.
Sentiment:
```

The model can answer correctly even without examples because it already understands the task pattern.

## Why zero-shot works

Modern LLMs are trained on large amounts of data and further tuned to follow instructions.

Important ideas:

- pretraining gives the model broad language and task knowledge
- instruction tuning makes the model better at following direct task requests
- alignment methods such as RLHF improve how well the model responds in human-preferred ways

Practical takeaway:

- zero-shot works best when the task is common, clear, and already familiar to the model

## When zero-shot is a good fit

- simple classification
- straightforward extraction
- summarization
- translation
- direct question answering from clear context

## When zero-shot may fail

- unusual label spaces
- strict formatting requirements
- domain-specific tasks
- multi-step reasoning
- tasks where the desired output style is very specific

## What to try before giving examples

If zero-shot underperforms, first improve:

- the clarity of the instruction
- the separation of instruction and input
- the output indicator or format constraint

If that is still not enough:

- move to few-shot prompting

## Zero-shot vs few-shot

Zero-shot:

- no demonstrations
- simpler and faster to write
- good first baseline

Few-shot:

- includes examples
- stronger when exact behavior or exact formatting matters
- useful when zero-shot is inconsistent

## My summary

- zero-shot means direct prompting without examples
- it works because modern LLMs are already trained to understand many task patterns
- it is usually the best starting baseline
- if the task is ambiguous or the output format is strict, few-shot may be better

## Self-test

1. What makes a prompt zero-shot?
2. Why can a model do sentiment classification without examples?
3. When should you try few-shot instead of staying zero-shot?
4. Does adding format instructions break zero-shot prompting?

## Review result

First self-test passed on 2026-04-02.

What I got right:

- zero-shot means no demonstrations or examples in the prompt
- modern models can do common tasks zero-shot because of pretraining and instruction tuning
- adding output constraints such as JSON format does not stop a prompt from being zero-shot
- if zero-shot output does not reliably meet task requirements, few-shot is the next step

Pass judgment:

- understanding is sufficient to move on
- key boundary is clear: examples determine zero-shot vs few-shot, not output constraints

## Next step

Move on to Few-Shot after passing the zero-shot concept check.
