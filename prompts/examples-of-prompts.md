# Examples of Prompts

This file collects prompt examples from the "Examples of Prompts" section and turns them into a reusable review set.

Use it for:

- quick recall of common prompt patterns
- comparing task types
- spotting why a prompt works or fails
- adapting the examples into your own experiments

## How to read these examples

For each example, focus on four questions:

1. What task is the model being asked to do?
2. What prompt elements are present?
3. Why does the current prompt work or fail?
4. What small change would improve it?

## 1. Information Extraction

### Goal

Pull a specific piece of information out of a paragraph.

### Prompt

```text
Author-contribution statements and acknowledgements in research papers should state clearly and specifically whether, and to what extent, the authors used AI technologies such as ChatGPT in the preparation of their manuscript and analysis. They should also indicate which LLMs were used. This will alert editors and reviewers to scrutinize manuscripts more carefully for potential biases, inaccuracies and improper source crediting. Likewise, scientific journals should be transparent about their use of LLMs, for example when selecting submitted manuscripts.

Mention the large language model based product mentioned in the paragraph above:
```

### Expected output style

```text
The large language model based product mentioned in the paragraph above is ChatGPT.
```

### Why it works

- the task is explicit: `Mention the large language model based product`
- the target information is present in the paragraph
- the output is narrow enough that the model can answer directly

### What to learn

- extraction can work with very simple prompting
- you do not always need complex formatting to get useful results

### How to improve it

If you want more reliable downstream use, constrain the output format more tightly:

```text
Extract the large language model based product mentioned in the paragraph.
Return only the product name.
```

## 2. Question Answering

### Goal

Answer a question from a provided context and avoid hallucinating beyond the text.

### Prompt

```text
Answer the question based on the context below. Keep the answer short and concise. Respond "Unsure about answer" if not sure about the answer.

Context: Teplizumab traces its roots to a New Jersey drug company called Ortho Pharmaceutical. There, scientists generated an early version of the antibody, dubbed OKT3. Originally sourced from mice, the molecule was able to bind to the surface of T cells and limit their cell-killing potential. In 1986, it was approved to help prevent organ rejection after kidney transplants, making it the first therapeutic antibody allowed for human use.

Question: What was OKT3 originally sourced from?

Answer:
```

### Expected output style

```text
Mice.
```

### Why it works

- the instruction says to answer from the context
- the prompt defines a fallback: `Unsure about answer`
- the answer style is constrained: `short and concise`

### What to learn

- QA prompts improve when you separate context, question, and answer clearly
- fallback rules reduce unsupported guessing

### How to improve it

If you want stricter grounding:

```text
Answer using only the context below.
If the answer is not stated in the context, respond exactly: Unsure about answer.
Use one short phrase.
```

## 3. Text Classification

### Goal

Map text into a fixed label set.

### Basic prompt

```text
Classify the text into neutral, negative or positive.

Text: I think the food was okay.
Sentiment:
```

### Expected output style

```text
Neutral
```

### Why it works

- the label space is given
- the task is simple and common
- the input and output indicator are clear

### What to learn

- basic classification often works with just instruction + input + output indicator

### Better prompt for exact formatting

```text
Classify the text into neutral, negative or positive.

Text: I think the vacation is okay.
Sentiment: neutral

Text: I think the food was okay.
Sentiment:
```

### Expected output style

```text
neutral
```

### Why this version is better

- it teaches the exact casing and label format by example
- it reduces ambiguity around output style

### Failure case

```text
Classify the text into nutral, negative or positive.

Text: I think the vacation is okay.
Sentiment:
```

### What to notice

- the model may ignore the misspelled label `nutral`
- it tends to fall back to the more familiar label `Neutral`

### What to learn

- model priors can override your intended label schema
- if your label space is unusual, define it more carefully and add examples

### Better fix direction

```text
Classify the text into one of these exact labels only:
- nutral = neither clearly positive nor negative
- negative = expresses dislike or dissatisfaction
- positive = expresses liking or satisfaction

Text: I think the vacation is okay.
Sentiment: nutral

Text: I think the food was okay.
Sentiment:
```

## 4. Conversation

### Goal

Control the assistant's tone, style, or persona in a chat.

### Technical assistant prompt

```text
The following is a conversation with an AI research assistant. The assistant tone is technical and scientific.

Human: Hello, who are you?
AI: Greeting! I am an AI research assistant. How can I help you today?
Human: Can you tell me about the creation of blackholes?
AI:
```

### Why it works

- it gives the assistant a role
- it provides a behavioral style
- it uses dialogue examples to anchor the pattern

### Simpler assistant prompt

```text
The following is a conversation with an AI research assistant. The assistant answers should be easy to understand even by primary school students.

Human: Hello, who are you?
AI: Greeting! I am an AI research assistant. How can I help you today?
Human: Can you tell me about the creation of black holes?
AI:
```

### What to learn

- role prompting can steer identity and tone
- audience framing changes complexity level
- examples in conversation format reinforce the intended behavior

### How to improve it

If you want even more accessible answers:

```text
The following is a conversation with an AI research assistant.
The assistant explains science in simple words for primary school students.
Use short sentences and one everyday analogy when helpful.
```

## 5. Code Generation

### Goal

Generate code from intent, comments, or structured requirements.

### Simple code prompt

```text
/*
Ask the user for their name and say "Hello"
*/
```

### What to learn

- LLMs can infer the programming task from comments alone
- simple code generation may not require explicitly naming the language

### Risk to remember

- if language, runtime, or style matters, say so explicitly

### More structured code prompt

```text
"""
Table departments, columns = [DepartmentId, DepartmentName]
Table students, columns = [DepartmentId, StudentId, StudentName]
Create a MySQL query for all students in the Computer Science Department
"""
```

### Why it works

- schema context is included
- the target SQL dialect is specified
- the task is concrete and testable

### What to learn

- structured inputs help code generation a lot
- giving schema context is often more important than verbose prose

## 6. Reasoning

### Goal

Solve a problem that requires intermediate thinking steps.

### Basic arithmetic prompt

```text
What is 9,000 * 9,000?
```

### What to learn

- simple arithmetic may work directly
- harder reasoning tasks often do not

### Weak reasoning prompt

```text
The odd numbers in this group add up to an even number: 15, 32, 5, 13, 82, 7, 1.

A:
```

### What to notice

- the model can answer incorrectly
- direct answers on reasoning tasks are less reliable

### Improved reasoning prompt

```text
The odd numbers in this group add up to an even number: 15, 32, 5, 13, 82, 7, 1.

Solve by breaking the problem into steps. First, identify the odd numbers, add them, and indicate whether the result is odd or even.
```

### Why it works better

- it forces the model to structure the task
- it reduces the chance of skipping key steps

### What to learn

- reasoning often improves when the prompt asks for an explicit process
- step-by-step structure can be more important than extra context

## Cross-example takeaways

Across all examples, the recurring pattern is:

- define the task clearly
- provide the relevant context
- make the output format explicit when needed
- add examples when the exact format matters
- break reasoning into steps when the task is error-prone

## Reusable prompt patterns

### Extraction pattern

```text
Extract [target information] from the text below.
Return only [desired format].

Text: [input]
```

### QA with grounding pattern

```text
Answer the question using only the context below.
If the answer is not stated, respond exactly: [fallback].

Context: [context]
Question: [question]
Answer:
```

### Classification pattern

```text
Classify the text into [label set].

Text: [input]
Label:
```

### Classification with exact formatting pattern

```text
Classify the text into one of these exact labels only: [labels].

Text: [example input]
Label: [example label]

Text: [input]
Label:
```

### Conversation pattern

```text
The following is a conversation with [role].
The assistant should [behavior/tone/audience constraint].

Human: [message]
AI:
```

### Code generation pattern

```text
[requirements, schema, or comment]
Create [language / query / function] that does [task].
```

### Reasoning pattern

```text
Solve the problem step by step.
First, [step 1].
Then, [step 2].
Finally, [step 3 / answer format].
```

## Review questions

1. Which examples rely mostly on instruction only?
2. Which ones become much better when output format is constrained?
3. Which examples benefit most from adding examples?
4. Which tasks become more reliable when broken into steps?
5. Which prompts would be risky in production without a fallback rule?
