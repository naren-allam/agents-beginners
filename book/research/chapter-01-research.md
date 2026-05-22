# Chapter 01 Research: Understanding Agents: The Core Concept

Research date: 2026-05-22

## Source List

- LangChain, "What's new in LangChain v1": https://docs.langchain.com/oss/python/releases/langchain-v1
- LangChain, "Agents": https://docs.langchain.com/oss/python/langchain/agents
- LangChain Python reference, `langchain.agents`: https://reference.langchain.com/python/langchain/agents/
- LangGraph, "Graph API overview": https://docs.langchain.com/oss/python/langgraph/graph-api
- LangGraph Python reference, `StateGraph`: https://reference.langchain.com/python/langgraph/graph/state/StateGraph/
- Russell and Norvig, *Artificial Intelligence: A Modern Approach*, Chapter 2 PDF: https://aima.cs.berkeley.edu/4th-ed/pdfs/newchap02.pdf
- IBM, "What Is a Simple Reflex Agent?": https://www.ibm.com/think/topics/simple-reflex-agent
- Red Hat, "Understanding AI agent types: A guide to categorizing complexity": https://www.redhat.com/en/blog/understanding-ai-agent-types-simple-complex
- NetLogo Models Library, "Thermostat": https://ccl.northwestern.edu/netlogo/models/Thermostat
- iRobot Create 3 hardware overview: https://ros.ncnynl.com/en/create3/hw/overview
- Rspamd Documentation, "Understanding Rspamd": https://docs.rspamd.com/getting-started/understanding-rspamd
- Rspamd architecture documentation: https://fatalbanana.github.io/rspamd.com/doc/architecture/index.html

## Key Findings

### Foundational Agent Definition

Russell and Norvig define an agent as anything that can be viewed as perceiving its environment through sensors and acting upon that environment through actuators. The same source distinguishes the agent function, which maps percept sequences to actions, from the agent program, which implements that function in code. This chapter should use the perception-action loop as the core professional mental model.

Important terms:

- Percept: the agent's input at a given moment.
- Percept sequence: the history of percepts observed so far.
- Agent function: the abstract mapping from percept sequence to action.
- Agent program: the concrete implementation of that mapping.
- Performance measure: the criterion used to judge whether the agent is doing well.

### Simple Reflex and Rule-Based Agents

IBM, Red Hat, and NetLogo all frame simple reflex agents as current-observation systems governed by condition-action rules. They do not store history or plan future consequences. Examples include thermostats, safety shutoff systems, simple email auto-responders, and traffic-control rules.

The NetLogo thermostat model is useful because it shows both the strength and limitation of simple rules: the thermostat can turn heating on below a target and off at or above a target, but more sophisticated control may need history or anticipation.

### Reactive vs. Deliberative Agents

Reactive agents select actions from the current percept or current state, usually through fast rules. Deliberative agents maintain some internal representation of the world, goals, or possible future actions. Chapter 1 should introduce this distinction gently and reserve deeper planning, self-correction, and multi-agent coordination for later chapters.

### Real-World Examples

The iRobot Create 3 hardware overview gives a concrete everyday robotics example. It describes proximity sensors, cliff sensors, wheel encoders, optical odometry, an IMU, buttons, LEDs, wheels, and charging contacts. This maps cleanly to sensors, actuators, environment, and goal-driven behavior.

Rspamd provides a software example. It analyzes email messages with authentication checks, content analysis, statistical classification, reputation systems, fuzzy hashing, and machine learning. Its action choices include no action, greylist, add header, rewrite subject, soft reject, and reject. This is a useful example of a software agent because it observes a message, scores evidence, and recommends an action to the mail transfer agent.

### LangChain 1.0 Notes

Official LangChain documentation states:

- `create_agent` is the standard way to build agents in LangChain 1.0.
- `create_agent` replaces `langgraph.prebuilt.create_react_agent` for the high-level LangChain agent API.
- The import path is `from langchain.agents import create_agent`.
- Agents combine language models with tools and run until a stop condition is met, such as a final output or iteration limit.
- Tools can be plain Python functions or decorated with `from langchain.tools import tool`.
- LangChain agents are built on LangGraph and inherit production features such as persistence, streaming, human-in-the-loop support, and time travel.

Common assumption to flag: older examples using `langgraph.prebuilt.create_react_agent` as the default high-level starting point are outdated for LangChain 1.0. Use `create_agent` from `langchain.agents` for introductory LangChain snippets.

### LangGraph 1.0 Notes

Official LangGraph documentation states:

- `StateGraph` is the main graph class for stateful graph workflows.
- Import path: `from langgraph.graph import StateGraph`.
- Graphs are composed from state, nodes, and edges.
- Nodes are functions that receive state and return state updates.
- Edges define the next node or stopping condition.
- `START` and `END` are available from `langgraph.graph`.
- A graph must be compiled with `.compile()` before it can be invoked.
- The compiled graph supports methods such as `invoke()`, `stream()`, `ainvoke()`, and `astream()`.
- `StateGraph` accepts `state_schema`, optional `context_schema`, and optional `input_schema` / `output_schema`.

Version-specific note: `config_schema` is deprecated in favor of `context_schema`. Avoid `config_schema` in chapter code.

Version-specific note: the LangGraph docs state that higher-level `create_agent` does not support Pydantic state schemas. For introductory professional examples, use `TypedDict` state with `StateGraph`.

## Chapter Flow Synthesis

1. Open with a concrete "missed expectation" scenario: a program that answers once versus an agent that keeps observing, deciding, and acting.
2. Define the perception-action loop before introducing AI or LLMs.
3. Explain daily-life examples with sensors, actuators, goals, and feedback.
4. Contrast agents with traditional request-response software.
5. Introduce autonomy, perception/action, and goal-driven behavior as the three core components.
6. Show a simple LangGraph state machine as a rule-based agent.
7. Explain reactive versus deliberative behavior using thermostat and mail-filter examples.
8. Briefly connect the chapter to later chapters on agentic AI, multi-agent systems, and responsible guardrails.

## APIs Verified for Chapter Code

- `from langgraph.graph import StateGraph, START, END`
- `StateGraph(State)` with `State` defined as a `TypedDict`
- `graph.add_node(name, callable)`
- `graph.add_edge(start, end)`
- `compiled = graph.compile()`
- `compiled.invoke(input_state)`
- `from langchain.agents import create_agent`
- `from langchain.tools import tool`
- `@tool` for defining tool metadata
- `create_agent(model, tools=[...])`
- `agent.invoke({"messages": [{"role": "user", "content": "..."}]})`

## Open Questions or Conflicts

- No blocker. The outline is clear by section numbering. Minor indentation inconsistencies in Chapter 5 and Chapter 7 will be resolved by the explicit section numbers when those chapters are drafted.
- For Chapter 1, no critical API remains unverified.
