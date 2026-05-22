# Chapter 02 Research: From Agents to Agentic AI

Research date: 2026-05-22

## Source List

- LangChain, "Agents": https://docs.langchain.com/oss/python/langchain/agents
- LangChain Python reference, `create_agent`: https://reference.langchain.com/python/langchain/agents/create_agent
- LangChain, "What's new in LangChain v1": https://docs.langchain.com/oss/python/releases/langchain-v1
- LangGraph, "Persistence": https://docs.langchain.com/oss/python/langgraph/persistence
- LangGraph, "Interrupts": https://langchain-ai.github.io/langgraph/concepts/human_in_the_loop/
- Anthropic, "How we built our multi-agent research system": https://www.anthropic.com/engineering/multi-agent-research-system
- Anthropic, "Effective harnesses for long-running agents": https://www.anthropic.com/engineering/effective-harnesses-for-long-running-agents
- Russell and Norvig, *Artificial Intelligence: A Modern Approach*, Chapter 2 PDF: https://aima.cs.berkeley.edu/4th-ed/pdfs/newchap02.pdf
- Red Hat, "Understanding AI agent types: A guide to categorizing complexity": https://www.redhat.com/en/blog/understanding-ai-agent-types-simple-complex
- arXiv, "SELAUR: Self Evolving LLM Agent via Uncertainty-aware Rewards": https://arxiv.org/abs/2602.21158v2

## Key Findings

### From Basic Agents to AI Agents

Chapter 1 established agents as systems that perceive, choose actions, and act toward goals. Chapter 2 should explain the transition from rule-based or reactive agents to AI agents: the decision policy is no longer only hand-written rules. It can include learned models, language models, embeddings, ranking systems, planners, memory, evaluators, and feedback loops.

The professional framing should emphasize that AI does not remove the need for agent architecture. It changes where uncertainty enters the system and how the system must be observed, constrained, and evaluated.

### LangChain 1.0 Agent Loop

Official LangChain documentation states:

- Agents combine language models with tools to create systems that reason about tasks, decide which tools to use, and iteratively work toward solutions.
- `create_agent` is the standard production-ready agent implementation in LangChain 1.0.
- `create_agent` creates an agent graph that calls tools in a loop until a stopping condition is met.
- The agent loop alternates between model calls and tool execution. If the model response includes tool calls, tools execute and tool results are added to the message list. The loop repeats until no more tool calls are present.
- LangChain describes this as the ReAct pattern: reasoning plus acting.
- Tools can be plain Python callables or `BaseTool` instances; `from langchain.tools import tool` remains the documented decorator path.

API notes verified for code:

- `from langchain.agents import create_agent`
- `from langchain.tools import tool`
- `agent = create_agent("openai:gpt-5.4-mini", tools=[...], system_prompt="...")`
- `agent.invoke({"messages": [{"role": "user", "content": "..."}]})`

### LangGraph 1.0 State, Persistence, and Feedback

Official LangGraph persistence documentation states:

- LangGraph can save graph state as checkpoints when the graph is compiled with a checkpointer.
- Checkpoints are organized into threads, with `thread_id` as the primary key.
- Persistence enables human-in-the-loop workflows, conversational memory, time travel debugging, and fault-tolerant execution.
- `InMemorySaver` is useful for local development and testing, but durable production systems should use a persistent checkpointer.
- Interrupts pause graph execution and use persistence to resume later with external input.

API notes verified for code:

- `from langgraph.graph import StateGraph, START, END`
- `from langgraph.checkpoint.memory import InMemorySaver`
- `graph = builder.compile(checkpointer=checkpointer)`
- `config = {"configurable": {"thread_id": "..."}}`
- `compiled.invoke(input_state, config=config)`

Chapter 2 can use checkpointing conceptually to introduce feedback and durable execution, while reserving detailed human approval flows for Chapter 4.

### Learning Capabilities

Learning can mean several different things in agentic AI:

- Model training or fine-tuning before deployment.
- Retrieval and memory updates during operation.
- Feedback-driven improvement from human corrections, evaluations, or telemetry.
- Policy or tool-selection changes based on observed outcomes.

For professionals, clarify that most production "learning" should not mean silently rewriting model behavior during a user-facing workflow. It often means capturing feedback, evaluating it, and updating retrieval stores, prompts, policies, eval sets, or model weights through controlled processes.

### Decision-Making Under Uncertainty

Agents operate under uncertainty when inputs are incomplete, tool results conflict, model outputs are probabilistic, or future outcomes are unknown. Rational-agent framing from Russell and Norvig remains useful: select actions expected to maximize the performance measure given available evidence.

Recent research such as SELAUR treats uncertainty as a signal for learning and exploration in LLM agents. For this book, avoid overclaiming research results; use it to show that uncertainty is not merely a nuisance but can become a signal for escalation, exploration, evaluation, or retraining.

### Planning and Execution

Planning and execution are distinct capabilities:

- Planning decomposes a goal into steps, dependencies, or strategy.
- Execution performs actions, calls tools, checks results, and revises the path when reality diverges from the plan.

LangChain's `create_agent` supports iterative tool use. LangGraph supports explicit stateful workflows for cases where planning, execution, routing, and review need to be visible and controlled.

### Self-Correction and Feedback Loops

Feedback loops are central to agentic AI:

- Tool observation loop: action produces observation, which informs the next action.
- Critic/evaluator loop: output is checked against criteria before finalization.
- Human feedback loop: users or reviewers correct output or approve sensitive actions.
- Operational feedback loop: telemetry and evals reveal failure modes that become tests, policy updates, memory updates, or prompt changes.

Warn against unbounded "reflect until better" loops. Production systems need stopping conditions, budgets, risk thresholds, and observability.

### Interaction with Other Systems

Tools are the bridge between AI reasoning and operational systems. Professional readers should treat tools as API contracts:

- Clear schemas.
- Narrow permissions.
- Validation.
- Idempotency for side effects.
- Auditing.
- Rate limits and retries.

This chapter should introduce the concept and defer deep guardrails to Chapter 4.

### Real-World Examples

Anthropic's multi-agent research system is a strong production case study. It uses a lead researcher agent that plans, stores its plan in memory, spawns specialized subagents, receives findings, decides whether more research is needed, and then passes results to a citation agent. The case illustrates planning, parallel tool use, state management, iteration, and evaluation.

Virtual assistants, recommendation agents, and autonomous robotics fit the outline:

- Virtual assistants process natural language, context, preferences, and tool calls.
- Recommendation agents learn from behavior and make uncertain predictions under changing user context.
- Autonomous robots blend perception, planning, control, and safety constraints in physical environments.

## Chapter Flow Synthesis

1. Open with the difference between a rule-based alert router and an AI incident assistant that investigates, asks for evidence, and updates its path.
2. Explain the transition from rules to learned decision policies.
3. Discuss uncertainty as a design condition, not a defect.
4. Define agentic AI as goal-directed AI systems that use state, tools, planning, feedback, and action over time.
5. Show a LangChain `create_agent` snippet for tool-using AI behavior.
6. Show a LangGraph snippet with checkpointing to introduce stateful feedback and durable workflows.
7. Ground capabilities with Anthropic research-system case study plus virtual assistants, recommendation systems, and robotics.
8. Bridge to Chapter 3 on multiple agents collaborating.

## Open Questions or Conflicts

- No blocker. The official LangChain and LangGraph documentation is clear enough for Chapter 2 snippets.
- Avoid using deprecated `langgraph.prebuilt.create_react_agent` in Chapter 2. LangChain 1.0 uses `create_agent` as the high-level starting point.
- Avoid implying that production agents should update their own model weights live during task execution. Use controlled learning loops instead.
