# Basics of Prompting

## Status

Completed on 2026-04-01 after Feynman-style explanation and learner summary.

## Core idea

A prompt works better when it clearly tells the model what task to do and includes the information needed to do it.

## Key concepts to learn

- a prompt can include an instruction, a question, context, inputs, and examples
- simple prompts can work, but better results usually need clearer task framing
- prompt formatting affects how easily the model infers the task
- zero-shot prompting means asking directly without examples
- few-shot prompting means providing demonstrations in the prompt
- chat models may use `system`, `user`, and `assistant` roles

## What I now understand

- prompting is about structuring the task so the model knows what to do
- a stronger prompt can combine instruction, question, context, input, and examples
- few-shot examples help the model learn the expected format, rules, and style from demonstrations
- zero-shot is a good first attempt when the task is simple and the model likely already understands it
- prompt formatting helps the model infer whether the task is QA, classification, completion, or another pattern

## My summary

- A prompt should be structured to let the model know how to do a specific task.
- Useful prompt parts: `instruction`, `question`, `context`, `input`, `examples`.
- Few-shot prompting is especially useful when I want the model to follow a specific format, rule, or style.

## Self-check

Passed learner explanation check on 2026-04-01.

- raw continuation is not the same as a clear task instruction
- adding context reduces ambiguity
- examples act like demonstrations
- few-shot improves pattern following

## Next step

Move on to Prompt Elements and compare how different prompt parts affect output quality.
