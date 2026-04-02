# Prompt Engineering Lab

A course-to-code learning lab for studying prompt engineering through notes, experiments, and publishable artifacts.

![Python](https://img.shields.io/badge/Python-3.10%2B-blue)
![License](https://img.shields.io/badge/License-MIT-green)
![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen)

## Overview

This project follows a simple workflow: read, explain ideas in plain language, build small experiments, and turn the results into reusable learning materials.

## Learning Roadmap

| Chapter | Status | Notes | Notebook | One-line Takeaway |
| --- | --- | --- | --- | --- |
| Introduction | ✅ Done | [notes/introduction.md](./notes/introduction.md) | [notebooks/01_introduction_practice.ipynb](./notebooks/01_introduction_practice.ipynb) | Prompt engineering is the systematic design of inputs to reliably bring out LLM capability. |
| LLM Settings | ✅ Done | [notes/llm-settings.md](./notes/llm-settings.md) | [notebooks/02_llm_settings_practice.ipynb](./notebooks/02_llm_settings_practice.ipynb) | Model behavior depends not only on prompts but also on decoding settings and model configuration. |
| Basics of Prompting | ✅ Done | [notes/basics-of-prompting.md](./notes/basics-of-prompting.md) | - | Clear instructions, context, and examples make prompts more reliable than raw text alone. |
| Prompt Elements | ✅ Done | [notes/prompt-elements.md](./notes/prompt-elements.md) | [notebooks/03_prompt_elements_practice.ipynb](./notebooks/03_prompt_elements_practice.ipynb) | A strong prompt is built from reusable parts such as instruction, context, input data, and output indicator. |
| General Tips | ✅ Done | [notes/general-tips.md](./notes/general-tips.md) | [notebooks/04_general_tips_practice.ipynb](./notebooks/04_general_tips_practice.ipynb) | Start simple, be specific, separate instructions clearly, and say what to do instead of only what not to do. |
| Zero-Shot | ✅ Done | [notes/zero-shot.md](./notes/zero-shot.md) | [notebooks/05_zero_shot_practice.ipynb](./notebooks/05_zero_shot_practice.ipynb) | Zero-shot prompting asks the model to do a task directly without demonstrations, relying on its instruction-following ability. |
| Few-Shot | ✅ Done | [notes/few-shot.md](./notes/few-shot.md) | - | Few-shot provides labeled demonstrations so the model learns the task format and label space through in-context learning. |
| Chain-of-Thought | ⬜ | - | - | - |
| Self-Consistency | ⬜ | - | - | - |
| Tree of Thoughts | ⬜ | - | - | - |
| RAG | ⬜ | - | - | - |
| ReAct | ⬜ | - | - | - |
| Adversarial Prompting | ⬜ | - | - | - |
| AI Agents | ⬜ | - | - | - |

## Project Structure

```text
prompt-engineering-lab/
├── notes/
├── notebooks/
├── prompts/
├── projects/
├── .env.example
├── .gitignore
├── LEARNING_LOG.md
├── README.md
└── requirements.txt
```

## Quick Start

```bash
python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
cp .env.example .env
python -m ipykernel install --user --name prompt-engineering-lab
jupyter notebook
```

## Learning Log

Session-based notes live in `LEARNING_LOG.md`. Chapter-level completion status stays in the roadmap above.

- [Learning Log](./LEARNING_LOG.md)

## References

- [Prompt Engineering Guide](https://github.com/dair-ai/Prompt-Engineering-Guide)
- [Feynman Technique](https://fs.blog/feynman-technique/)
