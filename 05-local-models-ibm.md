# ATO – Local Models and Agent Harness Slots (IBM) Notes

Nov 9, 2026 · @Sushma

IBM's session argued you can run agent harnesses on local models, with every slot (model, voice, vision, tools, sub-agents) swappable.

## Why local models

The slide weighed four plus-points for running models on your own laptop or desktop: simplicity, control, privacy and cost (free). Model families shown included IBM Granite, Qwen, Meta, Google and Z.ai. Serving tools shown included llama.cpp, vLLM and SGLang.

## Agent harness slots

The harness has slots, and each slot is a Provider + model + Validator stack you can swap.

| Slot | Filled by |
| --- | --- |
| Model | Provider + LLM + Validator |
| Voice in | Provider + speech-to-text + Validator |
| Image in | Provider + vision model + Validator |
| Tools | Guardrail + Provider, branching to LLM or document-model steps |
| Skills | Guardrail + Provider, branching to Validator + LLM + Provider |
| Sub-agents | Explore, Plan, Code, each its own harness |

## granite-cli and the private workflow demo

**granite-cli** has four columns:

- **Launchers:** agent harnesses (logos, names not legible in my photo).
- **Capabilities:** Mellea, Agent Skills, MCP.
- **Models:** Granite LLMs, Vision, Speech, Embeddings, Switch, Docling, or a custom model.
- **Providers:** local or hosted, including llama.cpp, Ollama, OpenRouter, vLLM and SGLang.

**Example: private workflow.** The demo ran the Hermes Agent harness with voice mode, asked it to start a project called "ATO Demo" with its project-planning tools, and produced a construction cost breakdown (lumber, doors, windows, other) from a material list. Mellea's `@mellea.generative` decorators did the narrow steps: decide whether a table is a material list, find the top-level plan document, and categorize catalog documents. Some demo numbers were too blurry to record.
