# All Things Open – AI Agents in Production Notes

Oct 19, 2026 · @Sushma

## Session details

Fill in during the first few minutes of the talk.

| Field | Notes |
| --- | --- |
| Talk title |  |
| Speaker(s) and company |  |
| Track |  |
| Day, time and room |  |
| Slides or recording link |  |
| Speaker contact (LinkedIn, GitHub, Mastodon) |  |

## Traditional software vs LLMs

The talk framed the core shift as moving from **deterministic** systems (same input, same output) to **probabilistic** ones (same input, a range of likely outputs). The cell text below is my general summary of each contrast; edit it to match what the speaker said.

| Dimension | Traditional software | LLMs |
| --- | --- | --- |
| Behavior | Deterministic: same input always gives the same output | Probabilistic: outputs vary run to run, even with the same prompt |
| Logic / reasoning | Explicit rules and branches written by developers | Learned patterns; reasons in natural language and can handle ambiguity |
| Data handling | Strict schemas and typed inputs; malformed data fails | Tolerates messy, unstructured input like text, documents and images |
| Optimal data format | Structured formats such as tables, JSON and typed objects | Natural language and lightly structured text, such as Markdown with clear context |
| Performance | Fast and predictable latency, often milliseconds | Slower and variable latency, often seconds per call |
| Cost | Near-zero marginal cost per request | Pay per token; cost grows with context size and model choice |
| Autonomy | Does only what it was programmed to do | Can plan, choose tools and act on goals, so it needs guardrails |

What this means in production: test with evals over many runs instead of single assert checks, and design for variance with validation, retries and human review.

## Agents vs autonomous agents: expense report example

The speaker used an employee's expense report to show the step from LLM-orchestrated workflows to fully agentic behavior. In a workflow, the LLM runs steps a developer laid out. An autonomous agent decides its own steps to reach a goal.

&#91;embedded content: expense report · workflow vs agent loop\]

The workflow always runs the same four steps. The agent plans, acts, checks the result and adapts until the report is done. The step and tool names are my sketch, not the speaker's slide.

### Hallmarks of fully agentic behavior

The examples in brackets are mine, using the expense report case; edit them to match the speaker's illustrations.

- **Goal-directed:** works toward an outcome, not a fixed script (get the report approved and reimbursed correctly)
- **Multi-step:** breaks the task into steps and chains them (read receipts, check policy, route for approval)
- **Adaptive:** changes course when something unexpected happens (missing receipt, over-limit meal)
- **Uses memory:** keeps context across steps and runs (this employee's past reports, earlier exceptions)
- **Uses tools:** calls outside systems (receipt OCR, policy lookup, finance system, email to the employee)

## Levels of AI, agents and autonomy

The talk frames five levels, from a plain tool to a fully autonomous agent. Cost, risk and failure mode all rise with each level; the middle rows (copilot and agent) are where most production work lands.

| Level | Name | Example | Failure mode |
| --- | --- | --- | --- |
| 1 | Tool | Deterministic script or app | Does exactly what it was coded to do, nothing more |
| 2 | AI Assistant | Chat that answers questions | Wrong or stale answer; a human acts on it |
| 3 | AI Copilot | Suggests actions inside your workflow | Human rubber-stamps a bad suggestion |
| 4 | AI Agent | Plans and calls tools to finish a task | Wrong tool call, drift, runaway cost |
| 5 | Autonomous AI Agent | Runs unattended, e.g. Claude Code in a loop, OpenClaw | Irreversible action with no human in the loop |

- Level 4 adds tool use and multi-step planning, with a human still approving key steps.
- Level 5 removes the approval step, so guardrails, identity and observability have to carry the safety load.
- Rule of thumb from the session: pick the lowest level that solves the problem.

## Harness in AI

A harness is the layer around the LLM server that turns a model into agentic work. From the talk:

- **The LLM alone is stateless** and gives back a reflection of its training data.
- **The LLM server** wraps the model, keeps the conversation, and adds the history to every call from a cache.
- **The harness** sits around the LLM server and handles agent execution.
- **Control flow:** it manages the ReAct loop and puts guardrails around it.
- **Tools and skills:** it mediates between them.
- **Skills loading:** the harness puts each skill's short description into the LLM's context first. When a task needs that expertise, the LLM decides to load the full skill.
- **Boundary:** it manages the line between the LLM server and the outside world.
- **Context:** it does context engineering and fetches extra context when needed.
- **Observability:** it tracks what happened at each step.

**Agentic prompt + LLM + harness = agent**

Question for the talk: which parts did the speaker build themselves, and which came from a framework?

### Harness details from the slides

- **MCP:** a standard way for the harness to tell the LLM which tools exist and how to call them. The slides also flag A2A for agent-to-agent calls.
- **Skills:** packaged instructions the harness loads on demand, instead of stuffing everything into the prompt.
- **Memory:** useful, but a poisoning risk if bad content gets written back and trusted later.
- **Tracing:** OpenTelemetry (OTel) traces record each step the agent takes.
- **Personal vs enterprise harness:** personal agents run on a laptop; the talk's point is that they tend to graduate to a cloud harness for identity, audit and shared control.

## Loops

Loop engineering is how the engineer sets up an agent to keep working until a goal is met. From the talk:

- **Goal:** the engineer defines what success looks like.
- **Verification method:** a way to check each attempt against the goal.
- **Stopping condition:** when the loop ends, once the goal is judged or achieved.
- **Token cost:** a loop uses a large number of tokens, so the cost of each run matters.

The Karpathy loop is the example given. The agent keeps modifying one asset and tests each version against a rigid metric. It then saves the change if the score passes, or rejects it.

Question for the talk: what metric was used in the Karpathy example, and what was the stopping condition?

## Graphs, small models and multi-agent

- **Graphs:** explicit nodes and edges give you a deterministic backbone with AI at chosen decision points. Examples shown: LangGraph and Microsoft Agent Framework.
- **Small language models (SLMs):** cheaper and faster for high-volume steps. A hybrid design routes simple cases to an SLM and hard ones to a frontier model; the slide example was insurance claims.
- **Fine-tuning:** LoRA / QLoRA via PEFT, Axolotl or Unsloth.
- **Sub-agent delegation:** a larger model (Opus) writes the proposal and hands narrow sub-tasks to a small one (Haiku) to control cost.
- **Agent tokenomics:** KV-cache reuse is a big cost lever.

## Where to apply agents

The talk scored use cases on five dimensions. Agents shine where they bring one of six superpowers: volume, assembly, orchestration, vigilance, speed and access to expertise.

- **Speed example:** the Forrester stat on slow lead follow-up (30%, cited via cdpinstitute.org) and the HBR "60x" lead-response stat (hbr.org/2011/03/the-short-life-of-online-sales-leads).
- **Data and context:** the agent is only as good as what it can see.
- **Integration and complexity:** more systems means more failure points.
- **Risk and reversibility:** the cautionary example was the Feb 22, 2026 OpenClaw inbox-deletion tweet. Prefer reversible actions.

## Key takeaways

Write these last: the three to five ideas you'd repeat to your team tomorrow.

1.
2.
3.

Best quote or slide:

## Architecture and patterns

Capture how the speaker structures agents that actually run in production.

- **Agent shape:** single agent, orchestrator plus subagents, or fixed workflow with LLM steps?
- **Tool use:** how tools are defined and scoped, and whether MCP or custom APIs are used
- **Memory and state:** what persists between runs, and where it lives
- **Human in the loop:** which steps need approval, and how hand-off works
- **Deployment:** hosting, scaling, queues, timeouts and retries

Sketch or diagram notes:

**Pattern summary from the slides:** deterministic backbone (workflow or graph) + AI at decision points + a harness that owns identity, tool access, memory and tracing. Per-sub-task least privilege: each sub-agent gets only the access its step needs.

## Reliability, evaluation and observability

Note how they know the agent works, and how they find out when it doesn't.

- **Evals:** offline test sets, LLM-as-judge, regression suites run in CI
- **Success metrics:** task completion rate, accuracy, escalation rate, latency
- **Tracing and logging:** tools used to trace each step and tool call
- **Failure modes they've seen:** loops, hallucinated tool calls, silent wrong answers
- **Recovery:** fallbacks, retries, circuit breakers, rollback of model or prompt changes

**From the slides:**

- **Drift:** agent behavior changes over time as models, prompts and data change, so keep checking it.
- **Logging traces:** record every step, tool call and decision, not just the final answer.
- **OpenTelemetry:** the common standard for traces; backends mentioned include Grafana, Datadog and Langfuse.

## Security, guardrails, cost and governance

- **Permissions:** least-privilege tool access, sandboxing, secrets handling
- **Prompt injection:** how untrusted input (web pages, emails, documents) is isolated
- **Guardrails:** input and output filters, policy checks, actions blocked without approval
- **Cost control:** token budgets, model routing (small vs. large models), caching
- **Governance:** audit logs, data retention, compliance or review process

**From the slides:**

- **Principle of least privilege (PoLP):** applied per sub-task, not per agent.
- **Workload identity:** short-lived tokens instead of long-lived keys.
- **Guardrails:** on input, output and tool calls.
- **Governance tests:** Test 1 Accountability (who owns the agent's actions), Test 2 Constraints and boundaries, Test 4 Adoption and culture. Test 3 was not captured in my photos.
- **Governance takeaways:** the closing slide summarizes these; see the photos IMG\_6857 to IMG\_6859.

## Tools, frameworks and resources

Add a row for each framework, library, model, repo or paper the speaker names.

| Name | What it's for | Link | Worth trying? |
| --- | --- | --- | --- |
|  |  |  |  |
|  |  |  |  |
|  |  |  |  |
| LangGraph / Microsoft Agent Framework | Graph-based agent workflows |  |  |
| OpenTelemetry (Grafana, Datadog, Langfuse) | Tracing agent steps |  |  |
| Ollama, Hugging Face PEFT, Axolotl, Unsloth | Running and fine-tuning small models |  |  |
| MCP / A2A | Tool and agent-to-agent protocols |  |  |

## Questions and Q&A

Questions to ask if there's time:

- What was the hardest part of moving from prototype to production?
- How do you evaluate changes to prompts or models before shipping them?
- What's your rough cost per task, and how did you bring it down?
- When did you decide *not* to use an agent?

Answers and audience questions worth remembering:

-

## Action items

- [ ] Grab the slides or recording link
- [ ] Connect with the speaker
- [ ] Try one tool from the resources table
- [ ] Share the takeaways with my team
