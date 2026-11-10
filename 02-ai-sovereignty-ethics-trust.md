# ATO – AI Sovereignty, Ethics and Trust Notes

Nov 9, 2026 · @Sushma

Notes from one All Things Open talk on controlling your AI future, then why ethics and trust matter.

## AI sovereignty: control, not independence

Sovereignty means control over three things: where the model runs (which model, whose servers, which country), who can see your data (prompts, files, logs, retention, training use), and who can switch it off (pricing, deprecations, terms, access). The goal is not full independence; it is choosing how much control each workload needs.

**Why it came up:** companies hesitate to share their "secret sauce" with frontier providers, and some cannot use them at all (legally sensitive data such as PHI, or companies outside the US). The slide quoted Microsoft CEO Satya Nadella: a company should be able to use a model without giving up the knowledge that makes it unique.

**A dial, not a switch.** Moving down the tiers buys more control and adds operational responsibility (hosting, patching, evals).

| Tier | What it is | Examples from the slide |
| --- | --- | --- |
| 1 | Frontier API or business plan | No training under commercial terms |
| 2 | Frontier model in your cloud | Amazon Bedrock, Google Vertex AI, Microsoft Foundry |
| 3 | Open-weight models you host | vLLM, Ollama, llama.cpp |
| 4 | Small models on your own devices | Sensitive data never leaves |

- Match each workload to the tier its data requires; most organizations will run several tiers at once.
- Frontier vendors do sign BAAs for "HIPAA-ready" services, but with feature limits.
- Route by sensitivity: regulated data to models you control, everything else to whatever passes your evals at the lowest cost. Start with one workload, measure with your own evals, then expand.

## Trust and sentiment: adoption is up, goodwill is down

**Klarna:** in 2024 its AI "did the work of 700 agents" (2.3M conversations in month one). In 2025 the same CEO said the AI-first approach produced lower quality and that investing in human support is the future: "There will always be a human if you want."

Share of Americans agreeing "AI will have a negative effect on society" (YouGov):

| Wave | Agree |
| --- | --- |
| Dec 2024 | 34% |
| Mar 2025 | 40% |
| Apr 2025 | 41% |
| Jun 2025 | 47% |

Trust in AI companies (US, Edelman): 50% in 2019 to 35% in 2024.

Ages 13-28 saying "more harm than good" (Gallup): 36% to 47% in one year.

More concerned than excited (Pew): 37% in 2021 to 50% in 2025; only 10% more excited than concerned.

Meanwhile daily AI use rose from 22% to 31% (Pew).

## Ethical AI: responsibility, trust, potential, ROI

We have been pushing AI without regard to ethics. Ethical AI takes fairness, trust and human responsibility into account.

- **Clarifies responsibility:** Air Canada argued its chatbot was a separate legal entity. The tribunal said Air Canada is responsible for all information on its website. AI cannot claim responsibility; only humans can.
- **Trust is supported by safety:** the slide cited a July 21, 2026 report of OpenAI models escaping a sandboxed evaluation via a zero-day and reaching Hugging Face's production database, followed on July 30 by an Anthropic disclosure of three break-ins by Claude models during evaluations. Confession or capability demo, trust is eroded either way.
- **Unlocks human potential:** Ford rehired about 350 veteran engineers after AI alone did not deliver quality; CEO Jim Farley credited "gray beards + AI" with hundreds of millions of dollars of cost tailwind.
- **Great ROI:** IBM Institute for Business Value (915 executives): organizations spending over 10% of AI budgets on ethics saw 30% higher operating profit from AI than those spending 5% or less. McKinsey: responsible-AI investors reported efficiency gains (42%), more consumer trust (34%), better reputation (29%), fewer incidents (22%).
- **AI fatalism pushes speed at all costs:** "if we don't win the race, China will." The slide's answer: speed alone leads to posturing and AI washing; ethical AI is slower but reaches success faster.
- Only 5% believe AI development represents their interests (Quinnipiac, 2026). The closing quote, from Eric Schmidt's commencement speech: the question is whether you will have shaped artificial intelligence.

## Takeaways and sources

- Sovereignty is a dial: match each workload to the tier its data needs.
- Treat skills, tools, prompts and evals as portable IP, so switching models is configuration, not a rebuild.
- Value human superpowers: owning responsibility, soft skills, relational trust, authentic connection.
- A late slide recapped agents: they now give real value, and the speaker asked what takeaway you will leave with.

Sources shown on slides:

- [IBM: why investing in AI ethics makes business sense](https://www.ibm.com/think/news/why-investing-in-ai-ethics-makes-good-business-sense)
- [McKinsey: responsible AI trust maturity survey](https://www.mckinsey.com/capabilities/tech-and-ai/our-insights/tech-forward/insights-on-responsible-ai-from-the-global-ai-trust-maturity-survey)
- [McKinsey: the sovereign AI agenda](https://www.mckinsey.com/capabilities/tech-and-ai/our-insights/tech-forward/the-sovereign-ai-agenda-moving-from-ambition-to-reality)
