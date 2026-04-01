# Prompt Elements

## Status

Completed on 2026-04-02 after two rounds of self-test.

## Core idea

Many prompts are made from a small set of reusable elements. Learning to identify these parts makes prompts easier to design, debug, and improve.

## The four common elements

### 1. Instruction

The instruction tells the model what task to perform.

Examples:

- summarize the paragraph
- classify the sentiment
- translate the sentence into Chinese

Working rule:

- if the model is unclear about what to do, fix the instruction first

### 2. Context

Context is extra information that helps the model respond better.

Examples:

- background information
- business rules
- domain definitions
- examples that clarify the task

Working rule:

- if the task is ambiguous or domain-specific, add context

### 3. Input Data

Input data is the actual content the model should operate on.

Examples:

- a customer review
- a question from the user
- a paragraph to summarize

Working rule:

- keep the input clearly separated from the instruction

### 4. Output Indicator

The output indicator signals the kind of answer or format you want.

Examples:

- `Sentiment:`
- `Summary:`
- `Answer in JSON`
- `Return 3 bullet points`

Working rule:

- if the answer format is drifting, strengthen the output indicator

## Example breakdown

Prompt:

```text
Classify the text into neutral, negative, or positive

Text: I think the food was okay.

Sentiment:
```

Element mapping:

- instruction: `Classify the text into neutral, negative, or positive`
- input data: `Text: I think the food was okay.`
- output indicator: `Sentiment:`
- context: not included in this example

Expected answer:

- `neutral`

## Important takeaway

You do not need all four elements every time.

For simple tasks:

- instruction + input may be enough

For harder tasks:

- add context
- strengthen the output indicator
- optionally add examples as part of context

## How to use this in practice

When a prompt fails, inspect it with this checklist:

1. Is the instruction clear?
2. Is there enough context?
3. Is the input data clearly separated?
4. Is the output format explicitly indicated?

## My summary

- instruction defines the task
- context steers the model
- input data is the content being processed
- output indicator shapes the final answer format

## Self-test

Try to answer without looking back:

1. What is the difference between instruction and input data?
2. Why is context optional but sometimes important?
3. What problem does an output indicator solve?
4. In a classification prompt, which element usually contains the label space?

## Review result

First self-test passed on 2026-04-02.

What I got right:

- instruction defines the task and input data is the content being processed
- context is optional for simple tasks but useful for reducing ambiguity in harder ones
- output indicator is the first thing to strengthen when the answer format drifts
- a basic translation prompt may only need instruction, input data, and output indicator

Second self-test also passed on 2026-04-02.

Further understanding confirmed:

- audience framing such as `for an engineering manager` usually works as context
- when tone or policy alignment is weak, context is often the first thing to improve
- a prompt may contain role instructions in addition to the four common elements
- if output length or format drifts, strengthen the output indicator before changing other parts

## Next step

Move on to General Tips after completing the Prompt Elements exercises.
