# General Tips for Designing Prompts

## Status

Completed on 2026-04-03 after two rounds of self-test.

## Core idea

Good prompts are usually not produced in one shot. They improve through iteration, clearer instructions, and more precise constraints.

## The main tips

### 1. Start simple

Do not begin with a huge prompt full of rules and edge cases.

Better approach:

- start with a small prompt
- test the output
- add only the missing information
- break a large task into smaller subtasks when needed

Why this matters:

- simpler prompts are easier to debug
- iteration helps you see which change improved the result

### 2. Put the instruction clearly

Tell the model exactly what task to perform.

Examples of strong task verbs:

- write
- classify
- summarize
- translate
- extract
- order

Useful pattern:

```text
### Instruction ###
Translate the text below to Spanish:

Text: "hello!"
```

Why this matters:

- clear instructions reduce ambiguity
- separators such as `###` make the structure easier to follow

### 3. Be specific

Specificity means telling the model what kind of result you want.

Examples:

- weak: `Explain prompt engineering briefly.`
- stronger: `Use 2-3 sentences to explain prompt engineering to a high school student.`

Important nuance:

- specific does not mean long
- extra detail only helps if it is relevant to the task

### 4. Avoid imprecise wording

Vague constraints often lead to vague outputs.

Example of imprecise wording:

- `Keep the explanation short, only a few sentences, and don't be too descriptive.`

Better:

- `Use 2-3 sentences to explain the concept of prompt engineering to a high school student.`

Why this matters:

- the model follows concrete constraints better than fuzzy preferences

### 5. Prefer telling the model what to do

Prompts that only say what not to do often fail.

Weak pattern:

- `Do not ask for interests. Do not ask for personal information.`

Better pattern:

- define the allowed behavior
- define the fallback behavior
- define the response if the needed information is unavailable

Why this matters:

- positive instructions give the model a path to follow
- fallback behavior makes responses more robust

## Example takeaways

### Extraction example

Prompt:

```text
Extract the name of places in the following text.

Desired format:
Place: <comma_separated_list_of_places>
```

Why it works:

- the task is explicit
- the desired output format is explicit
- irrelevant freedom is reduced

### Movie agent example

Why the first version fails:

- it only blocks behavior
- it does not provide an alternative path

Why the improved version works better:

- it defines what the agent should recommend
- it defines what the agent should avoid
- it defines what to say when no recommendation is available

## My summary

- start small and iterate
- use clear task instructions
- be specific about the output you want
- remove vague wording
- tell the model what to do, not only what to avoid

## Practical checklist

When a prompt underperforms, ask:

1. Is the task instruction explicit?
2. Is the prompt specific enough?
3. Is any wording vague or subjective?
4. Am I telling the model what success looks like?
5. Should this be split into smaller subtasks?

## Self-test

1. Why is starting simple better than writing a giant prompt first?
2. What is the difference between being specific and being verbose?
3. Why do negative-only instructions often fail?
4. When should you split one big prompt into subtasks?

## Review result

First self-test passed on 2026-04-03.

What I got right:

- starting simple makes prompts easier to iterate and debug
- specificity adds relevant constraints, while verbosity often adds noise
- negative-only instructions are weaker because they do not define the desired path clearly
- large multi-part tasks should usually be split into smaller subtasks

Second self-test also passed on 2026-04-03.

Further understanding confirmed:

- weak prompts often fail because their constraints are vague, not only because they are too short
- a better rewrite should replace fuzzy wording like `briefly` with testable limits such as `2-3 sentences`
- for agent-like prompts, add both positive behavior and fallback behavior
- task splitting is the right move when one prompt contains several different objectives

## Next step

Move on to Zero-Shot after completing the rewrite exercises.
