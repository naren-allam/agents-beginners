# Chapter 03 Research: Multi-Agent Systems and Collaboration

Research date: 2026-05-22

## Source List

- LangChain, "Subagents": https://docs.langchain.com/oss/python/langchain/multi-agent/subagents
- LangChain, "Build a personal assistant with subagents": https://docs.langchain.com/oss/python/langchain/supervisor
- LangGraph Supervisor Python repository: https://github.com/langchain-ai/langgraph-supervisor-py
- LangChain blog, "Command: A new tool for building multi-agent architectures in LangGraph": https://blog.langchain.com/command-a-new-tool-for-multi-agent-architectures-in-langgraph
- arXiv, "Multi-Agent Coordination across Diverse Applications: A Survey": https://arxiv.org/html/2502.14743v2
- Anthropic, "How we built our multi-agent research system": https://www.anthropic.com/engineering/multi-agent-research-system
- TrueFoundry, "What Are Multi-Agent Systems?": https://www.truefoundry.com/blog/multi-agent-systems
- Famaey et al., "Hybrid Multiverse-based Parallel Computing": https://www.famaey.eu/papers/cnf-mokhtari2025a.pdf
- SynthetIQ Solutions, "Multi-Agent Frameworks for Operational AI Systems": https://synthetiq.solutions/multiagent-whitepaper.html

## Key Findings

### Multi-Agent Systems Definition

Multi-agent systems consist of multiple autonomous or semi-autonomous agents operating in a shared environment. They may cooperate toward a shared objective, compete for resources, negotiate task boundaries, or coordinate to avoid conflicts.

The 2025 coordination survey defines coordination as agents working together, communicating, and adjusting actions to achieve common goals while avoiding conflict and optimizing system-level performance. This is the key professional distinction: a multi-agent system is not merely "many prompts." It is a coordination architecture.

### Why Multiple Agents

Use multiple agents when work naturally decomposes by expertise, tool domain, context boundary, trust boundary, latency profile, or organizational responsibility. Do not use multiple agents simply because it sounds more advanced.

Common reasons:

- Context isolation: each subagent has a focused context window.
- Tool partitioning: each subagent owns a smaller, safer tool set.
- Specialization: prompts and evaluation can be tuned by role.
- Parallelism: independent subtasks can run concurrently.
- Governance: sensitive domains can have separate approval and audit paths.

Trade-offs:

- More coordination overhead.
- Higher latency and cost.
- More failure modes in routing, duplication, handoff, and synthesis.
- Harder observability and evaluation.

### LangChain 1.0 Subagents Pattern

Official LangChain documentation recommends the subagents architecture: a central main agent, often called a supervisor, coordinates subagents by calling them as tools.

Verified API pattern:

- `from langchain.agents import create_agent`
- `from langchain.tools import tool`
- Create a subagent using `create_agent(model=..., tools=[...])`.
- Wrap the subagent in a tool with `@tool("name", description="...")`.
- Inside the tool, call `subagent.invoke({"messages": [{"role": "user", "content": query}]})`.
- Return `result["messages"][-1].content`.
- Create the main agent with the wrapper tools.

Official characteristics:

- Centralized control.
- Subagents return to the main agent rather than interacting directly with the user.
- Subagents are invoked via tools.
- Parallel execution is possible when the main agent invokes multiple subagents in one turn.
- A supervisor differs from a router: a router is a classification/dispatch step; a supervisor is a full agent maintaining conversation context across turns.

Important version-specific note: the `langgraph-supervisor-py` repository says the team now recommends using the supervisor pattern directly via tools for most use cases because it gives more control over context engineering. The separate supervisor library remains available for compatibility and specific use cases, but Chapter 3 should use the official direct subagent-as-tool pattern for code.

### LangGraph Handoffs and Command

LangGraph's `Command` concept lets a node return both a state update and a target node, enabling direct handoffs and dynamic multi-agent architectures. This is useful for peer-to-peer and hierarchical systems where control should move between actors at runtime.

For Chapter 3, introduce `Command` conceptually but avoid code unless the current official API reference is separately verified. The chapter can focus code on the documented LangChain 1.0 subagent tool pattern.

### Centralized vs. Decentralized Control

Centralized or supervisor architectures route work through a main coordinator. They are easier to observe, easier to govern, and useful when one component must own final synthesis.

Decentralized or swarm architectures allow agents to hand off directly to one another. They can better match distributed environments but are harder to audit and debug.

Pipeline architectures use a fixed sequence of agents and fit predictable workflows like document extraction -> compliance review -> summary.

Hybrid systems combine centralized global allocation with distributed local refinement, especially in robotics and large-scale operations.

### Conflict Resolution and Negotiation

Conflict appears when agents compete for resources, produce contradictory outputs, duplicate work, or choose incompatible actions. Resolution mechanisms include:

- Priority rules.
- Shared state and locks.
- Task ownership.
- Consensus protocols.
- Auctions or bidding for resource allocation.
- Human arbitration for high-risk disagreements.
- Explicit negotiation messages or proposals.

Professionally, conflict resolution should be designed into the architecture rather than left to natural-language improvisation.

### Scaling Multi-Agent Systems

Scaling concerns:

- Cost grows with number of agents, model calls, and tool calls.
- Latency grows unless independent work is parallelized.
- Context grows unless subagents have isolated scopes.
- Observability becomes harder without trace IDs and structured state.
- Reliability depends on retries, idempotency, checkpointing, and clear termination.

### Real-World Examples

Anthropic's multi-agent research system is a current production example. It uses a lead agent that plans, persists strategy, creates specialized subagents, receives findings, decides whether additional research is needed, and sends results to a citation agent.

Robotics and swarm examples show the need for coordination in physical environments. The hybrid swarm scheduling paper combines centralized global task allocation with local distributed schedule refinement, illustrating why hybrid architectures can outperform pure centralized or pure decentralized approaches.

Enterprise workflow examples include document processing, benefits processing, order-to-cash, incident response, and analyst workflows where specialist agents handle research, compliance, extraction, verification, and synthesis.

## Chapter Flow Synthesis

1. Open with a professional workflow where one agent becomes overloaded by too many tools and responsibilities.
2. Define multi-agent systems as coordination architectures, not collections of chatbots.
3. Explain multiple goals, communication, coordination, and cooperation.
4. Compare supervisor, decentralized, pipeline, and hybrid architectures.
5. Show a LangChain 1.0 supervisor/subagents code snippet using subagents as tools.
6. Discuss conflict resolution and negotiation with operational examples.
7. Discuss scaling concerns: cost, latency, state, observability, evaluation.
8. Ground with robotics, simulations, Anthropic research system, and enterprise workflows.
9. Bridge to Chapter 4 on safety and responsible AI.

## Open Questions or Conflicts

- No blocker for Chapter 3.
- Use the direct LangChain subagent-as-tool pattern in code, not `langgraph-supervisor`, because official repository guidance recommends the direct tool-based supervisor pattern for most use cases.
- Avoid presenting decentralized swarms as inherently superior. They trade central bottlenecks for harder governance and observability.
