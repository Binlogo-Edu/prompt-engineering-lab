# Few-Shot Prompting

## Status

Completed on 2026-04-02 after concept check.

## Core idea

Few-shot prompting means providing a small number of input-output demonstrations inside the prompt to guide the model toward the desired behavior, format, or label space.

## Definition

A few-shot prompt includes:

- an instruction or task description (optional but common)
- one or more labeled examples (demonstrations)
- the actual input where the model should generate a response

Simple 1-shot example:

```text
A "whatpu" is a small, furry animal native to Tanzania. An example of a sentence that uses the word whatpu is:
We were traveling in Africa and we saw these very cute whatpus.

To do a "farduddle" means to jump up and down really fast. An example of a sentence that uses the word farduddle is:
```

The model learns the task pattern from the single example and applies it to the new input.

## Why few-shot works

This is called **in-context learning**: the model uses the demonstrations as conditioning to infer what the task requires, without any weight updates.

Key factors:
- the label space seen in examples matters (even if labels are randomly assigned)
- the input distribution shown in examples matters
- the format of the demonstrations matters
- even random labels help more than no labels at all

This means format and distribution are load-bearing parts of the prompt, not just the correctness of the labels.

## What counts as few-shot

- 1-shot: one example
- few-shot: typically 3–10 examples
- more examples generally improve reliability, up to a point

## Tips for writing good demonstrations (Min et al. 2022)

1. Label space matters: use the same set of labels you want in the output
2. Input distribution matters: examples should look similar to real inputs
3. Format consistency helps, but newer models are becoming more robust to format variation
4. Random labels still beat no labels — the format and distribution signal is what the model needs most
5. Labels from the true distribution (not uniform random) perform better

## Limitations of few-shot prompting

Few-shot prompting improves on zero-shot for many tasks but breaks down on **multi-step reasoning**.

Example failure: asking whether odd numbers in a list sum to an even number.

Even with 4-5 examples showing the correct answer format, the model still gets the answer wrong — because it cannot reliably carry out the intermediate arithmetic steps.

Why: the demonstrations only show the final answer. They do not show the reasoning steps. The model is not learning *how* to solve the problem, just *what kind of answer* to give.

## When to use few-shot

- the task format or output style is specific and hard to describe by instruction alone
- zero-shot gives inconsistent or wrong-format outputs
- the label space is non-standard
- the task is classification, extraction, or generation with a clear pattern

## When few-shot is not enough

- multi-step arithmetic or logic
- commonsense reasoning that requires intermediate steps
- tasks where the model needs to show its work to get the right answer

In these cases, the next technique is **chain-of-thought prompting**, where the demonstrations include the reasoning steps, not just the final answer.

## Few-shot vs zero-shot summary

| | Zero-shot | Few-shot |
|---|---|---|
| Examples in prompt | None | 1 or more |
| Format signal | Instruction only | Demonstrated by examples |
| Complexity limit | Simple tasks | Moderate tasks |
| Token cost | Low | Higher |
| When it fails | Unusual format, weak instruction | Multi-step reasoning |

## My summary

- few-shot prompting adds labeled examples to help the model understand the task format and label space
- in-context learning does not change model weights — the examples act as soft conditioning
- format and distribution in the examples matter more than label correctness
- few-shot is a strong step up from zero-shot for classification and pattern tasks
- it still fails on multi-step reasoning, which requires chain-of-thought prompting

## Self-test

1. What is the difference between zero-shot and few-shot prompting?
2. Why do even randomly labeled examples still help?
3. What two properties of the demonstrations matter most (Min et al. 2022)?
4. Why does few-shot fail on the odd-number-sum reasoning task?
5. What technique is recommended after few-shot fails on reasoning?

## Review result

Self-test passed on 2026-04-02.

What I got right:

- zero-shot vs few-shot: the presence or absence of demonstrations
- random labels still help because the model learns the label space and input distribution, not the correctness of each label
- few-shot fails on reasoning because demonstrations only show final answers, not intermediate steps — the model learns output format but not how to solve the problem
- representativeness of examples matters more than quantity; 3–10 examples is the typical range
- Chain-of-Thought is the recommended next technique

Key insight from discussion:

- "模型学的是范式，不是答案" — the model extracts pattern and format from demonstrations, not ground truth labels
- the missing ingredient in the reasoning failure is the intermediate steps, not the problem description

## Next step

Move on to Chain-of-Thought (CoT) prompting.
