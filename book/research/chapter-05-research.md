# Chapter 05 Research: Transitioning from Traditional Engineering to Agentic AI

Research date: 2026-05-22

## Source List

- LangChain, "Quickstart": https://docs.langchain.com/oss/python/langchain/quickstart
- LangChain, "Agents": https://docs.langchain.com/oss/python/langchain/agents
- LangChain, "Tools": https://docs.langchain.com/oss/python/langchain/tools
- LangGraph, "Graph API overview": https://docs.langchain.com/oss/python/langgraph/graph-api
- Augment Code, "Agentic Engineering Operating Model: Teams + Agents": https://www.augmentcode.com/guides/agentic-engineering-operating-model
- arXiv, "A Practical Guide to Agentic AI Transition in Organizations": https://arxiv.org/html/2602.10122v1
- Harness Engineering, "AI Agents Just Went From Chatbots to Coworkers": https://harness-engineering.ai/blog/ai-agents-just-went-from-chatbots-to-coworkers-what-engineering-teams-must-build-now/
- Ranjan Kumar, "State Management for Agentic Systems": https://ranjankumar.in/harness-engineering-state-management-agentic-systems-checkpoint-memory
- "From AI Agent Demo to Production System: A Full-Stack Blueprint": https://dzds.me/posts/20260502-building-production-agent-products/
- EngineersOfAI, "Agentic AI - Engineering Track": https://engineersofai.com/docs/agentic-ai/home

## Key Findings

### From If-Else to Goal-Driven Systems

Traditional engineering commonly decomposes behavior into deterministic flows: request, validation, branch, function call, response. Agentic engineering introduces loops that observe, decide, act, evaluate, and continue until a stopping condition is met.

The professional transition is not abandoning deterministic engineering. It is deciding where determinism should remain and where model-mediated action selection provides value. Good production agents are hybrids: deterministic boundaries, typed tools, explicit state, traceable loops, and model-mediated interpretation or planning where rules would become brittle.

### LangChain 1.0 Notes

Official LangChain quickstart and agents docs verify:

- `create_agent` is the standard way to create an agent.
- Import path: `from langchain.agents import create_agent`.
- Tools can be Python functions or decorated with `from langchain.tools import tool`.
- Tool names, descriptions, and argument names become part of the model's prompt.
- Type hints are required for tool schemas.
- The agent is invoked with `agent.invoke({"messages": [{"role": "user", "content": "..."}]})`.
- LangChain agents are built on LangGraph and can use tracing/evaluation through LangSmith.

### LangGraph 1.0 Notes

Official LangGraph graph API verifies:

- `StateGraph` is the main graph class for stateful workflows.
- A graph is composed of state, nodes, edges, and compilation.
- Nodes receive state and return partial updates.
- Edges define routing and stopping behavior.
- `START` and `END` are imported from `langgraph.graph`.
- A graph must be compiled before invocation.
- Use `TypedDict` for simple state schemas.

### Building a First Agentic Project

The best first professional agentic project is:

- Low blast radius.
- Tool-light.
- Easy to evaluate.
- Connected to a real workflow.
- Capable of producing reviewable outputs.
- Measured against a current baseline.

Examples: incident summary drafting, support ticket routing, documentation retrieval, release-note generation, internal policy lookup, dependency update proposals, QA checklist generation.

Avoid first projects where the agent directly performs high-impact actions such as deleting data, moving money, changing production configuration, approving benefits, or communicating externally without review.

### Integrating Agents into Existing Systems

Production sources consistently emphasize that a production AI agent is not just an LLM with tools. It requires identity, permissions, durable state, memory, tool execution, safety, cost controls, observability, evaluation, and recovery.

Key integration principles:

- Treat tools as API contracts.
- Keep side effects idempotent.
- Store workflow state outside the model.
- Add checkpoint/resume for long-running tasks.
- Make human approval a first-class branch.
- Use traces before optimizing.
- Build cost envelopes and termination conditions.
- Integrate gradually into existing workflows instead of replacing entire processes.

### Measuring First Agent Impact

Measure before and after:

- Time saved.
- Task completion rate.
- Quality or correctness.
- Human review effort.
- Defect or incident rate.
- Escalation rate.
- Cost per task.
- User satisfaction.
- Policy violation rate.

Impact should include stability and risk, not only throughput.

### Skillsets for Agentic Engineers

Professional sources emphasize new skills:

- Agent architecture and loop design.
- Tool/API design for model-mediated use.
- State, memory, and checkpointing.
- Evaluation and trajectory analysis.
- Observability and tracing.
- Safety, permissions, and human-in-the-loop design.
- Cost and latency engineering.
- Product/domain collaboration.
- Prompt/context engineering.
- Multi-agent coordination.

### Bringing Teams Along

Organizational transition requires more than individual tool use. Sources emphasize platform foundations, governance, shared workflows, and collaboration between engineering and business-domain stakeholders. Human roles shift from manual execution toward steering, review, workflow design, and accountability.

The recommended adoption pattern:

1. Map an existing manual workflow.
2. Choose a low-risk agentic slice.
3. Build a controlled prototype.
4. Evaluate against baseline.
5. Add guardrails and observability.
6. Pilot with domain users.
7. Expand autonomy only after evidence.

## Chapter Flow Synthesis

1. Open with a professional scenario where an if-else workflow collapses under varied user intent.
2. Explain goal-driven systems as loops with state, actions, and stopping conditions.
3. Define agentic thinking as designing tools, state, evaluation, and human boundaries.
4. Introduce project patterns that fit existing systems.
5. Show a LangGraph code snippet for a small deterministic-to-agentic workflow.
6. Show a LangChain `create_agent` snippet for a narrowly scoped first project.
7. Discuss measuring impact and team upskilling.
8. Bridge to Chapter 6 on enterprise adoption.

## Open Questions or Conflicts

- No blocker. Official LangChain and LangGraph docs verify needed APIs.
- The outline has indentation inconsistencies in Chapter 5, but section numbering is clear. Preserve section numbers and names exactly.
- Avoid suggesting professionals abandon deterministic engineering. The correct framing is hybrid systems with deterministic controls and model-mediated flexibility.
