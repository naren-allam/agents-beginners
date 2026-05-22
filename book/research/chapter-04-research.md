# Chapter 04 Research: Ensuring Safe and Responsible Agentic AI

Research date: 2026-05-22

## Source List

- LangChain, "Guardrails": https://docs.langchain.com/oss/python/langchain/guardrails
- LangChain, "Human-in-the-loop": https://docs.langchain.com/oss/python/langchain/human-in-the-loop
- LangChain, "Middleware overview": https://docs.langchain.com/oss/python/langchain/middleware/overview
- LangGraph, "Interrupts": https://docs.langchain.com/oss/python/langgraph/interrupts
- LangGraph, "Persistence": https://docs.langchain.com/oss/python/langgraph/persistence
- LangSmith, "Evaluation": https://docs.langchain.com/langsmith/evaluation
- LangSmith product overview: https://langsmith.org/
- OWASP, "OWASP Top 10 for Agentic Applications": https://genai.owasp.org/download/52117/
- OWASP, "Securing Agentic Applications Guide": https://genai.owasp.org/download/49059/
- NIST, "Adversarial Machine Learning: A Taxonomy and Terminology of Attacks and Mitigations": https://nvlpubs.nist.gov/nistpubs/ai/NIST.AI.100-2e2025.pdf
- GDPR Article 5, "Principles relating to processing of personal data": https://www.legislation.gov.uk/eur/2016/679/article/5/adopted?view=plain
- HHS, "The HIPAA Privacy Rule": https://www.hhs.gov/hipaa/for-professionals/privacy
- HHS, "The HIPAA Security Rule": https://www.hhs.gov/hipaa/for-professionals/security/
- PCI Security Standards Council, "PCI DSS v4.0.1 Document Library": https://www.pcisecuritystandards.org/document_library/
- India, "Digital Personal Data Protection Act, 2023" overview and text references: https://www.meity.gov.in/
- LangSmith, "How to run a pairwise evaluation": https://docs.langchain.com/langsmith/evaluate-pairwise

## Key Findings

### Guardrails for Agentic AI

LangChain defines guardrails as safety checks and content filtering that validate and filter content at key points in agent execution. Common use cases include preventing PII leakage, detecting prompt injection, blocking harmful content, enforcing business rules, and validating output quality.

Guardrails can be deterministic or model-based:

- Deterministic guardrails use explicit rules, regex, keyword checks, permissions, schemas, and policy logic. They are fast and predictable, but may miss nuance.
- Model-based guardrails use models or classifiers to evaluate semantic risks. They can catch subtle issues but are slower, more expensive, and need evaluation.

LangChain 1.0 implements many guardrail patterns through middleware. Built-in examples include `PIIMiddleware` and `HumanInTheLoopMiddleware`.

### LangChain 1.0 API Notes

Verified imports and patterns:

- `from langchain.agents import create_agent`
- `from langchain.agents.middleware import PIIMiddleware, HumanInTheLoopMiddleware`
- `from langchain.tools import tool`
- `from langgraph.checkpoint.memory import InMemorySaver`
- `from langgraph.types import Command`
- `PIIMiddleware("email", strategy="redact", apply_to_input=True)`
- `HumanInTheLoopMiddleware(interrupt_on={"send_email": {"allowed_decisions": ["approve", "edit", "reject"]}})`
- `create_agent(..., middleware=[...], checkpointer=InMemorySaver())`
- HITL invocation requires a `config` with `{"configurable": {"thread_id": "..."}}`.
- With `version="v2"`, interrupt results include an `interrupts` attribute.
- Resume with `Command(resume={"decisions": [{"type": "approve"}]})`.

Important version-specific note: HITL middleware matches tool names. For Python `@tool` decorated functions, the tool name defaults to the function name unless otherwise configured.

Important production note: `InMemorySaver` is for testing/prototyping. Production systems should use a durable checkpointer such as a Postgres-backed saver.

### LangGraph Interrupts

LangGraph interrupts pause execution and wait for external input. The graph state is saved through the persistence layer. Resume is done by invoking the graph with `Command(resume=value)` using the same thread ID.

Use cases include approval workflows, reviewing and editing outputs or tool calls, validating human input, and pausing before critical API calls, database changes, or financial transactions.

### Agent Performance Evaluation

LangSmith evaluation distinguishes:

- Offline evaluation: pre-release evaluation on curated datasets to compare versions, benchmark, and catch regressions.
- Online evaluation: production monitoring on real user interactions, with filters and sampling rates to control cost.

Evaluator types include human review, code evaluators, LLM-as-judge, and pairwise comparison. Production traces can be added back to datasets to create a feedback loop: observe failure -> add trace to dataset -> evaluate fix offline -> redeploy.

Agentic systems should be evaluated by trajectories, not only final answers. Important metrics include final correctness, tool-call correctness, safety policy adherence, latency, cost, escalation rate, recovery after failure, and human override rate.

Pairwise and A/B-style evaluations are useful when a team needs to compare two prompts, model configurations, tool policies, or agent versions. LangSmith supports pairwise evaluation for comparing experiment outputs. In production, controlled A/B tests should be used carefully with risk-tiering: low-risk workflows can compare versions on live traffic, while high-risk workflows should use offline replay, shadow mode, or human review before exposure.

### Security and Compliance Frameworks

Relevant frameworks and laws include:

- GDPR: personal data processing principles such as lawfulness, fairness, transparency, purpose limitation, data minimization, storage limitation, and integrity/confidentiality.
- India's Digital Personal Data Protection Act, 2023 (DPDP Act / DPDPA): digital personal data processing, data fiduciary obligations, reasonable safeguards, breach notification, and erasure when purpose no longer applies.
- HIPAA: Privacy Rule protection for protected health information and Security Rule safeguards for electronic protected health information.
- PCI DSS: technical and operational controls for environments that store, process, or transmit payment account data.
- OWASP Agentic guidance: agent-specific risks such as excessive agency, tool misuse, memory manipulation, and orchestration flaws.
- NIST AI 100-2 E2025: adversarial machine learning taxonomy and shared terminology for attacks and mitigations.

Chapter 4 should not present these as legal advice. It should explain how agent architecture must provide evidence for compliance: data minimization, access control, logging, retention, redaction, approval, and auditability.

### Monitoring and Feedback in Production

LangSmith and similar observability systems capture traces: model calls, tool calls, inputs, outputs, decision points, costs, latency, and errors. The LangSmith overview emphasizes that traces are often the only record of what an agent did and why, because agents choose outputs and actions at runtime.

Production monitoring should cover:

- Tool-call sequences.
- Sensitive action attempts.
- Interrupt frequency and human decisions.
- Cost and latency.
- Error rates and retries.
- Policy violations.
- Drift in task mix and failure modes.

### Red Teaming Agentic AI

OWASP and NIST sources emphasize that agentic systems create new attack surfaces beyond model-only applications. Agentic risks include excessive agency, tool misuse, prompt injection, memory/context manipulation, authorization failures, orchestration flaws, cascading multi-agent failures, and unsafe autonomous planning.

OWASP guidance emphasizes "least agency": avoid unnecessary autonomy because it expands the attack surface without adding value. It also emphasizes observability as non-negotiable because teams need to know what agents are doing, why they are doing it, and which tools they invoke.

NIST AI 100-2 E2025 provides a taxonomy for adversarial machine learning, including attack lifecycle stages, attacker goals, capabilities, and mitigation terminology. Chapter 4 should use it to frame red teaming as systematic risk discovery, not ad hoc prompt poking.

### Chapter Flow Synthesis

1. Open with a realistic high-stakes failure scenario: an agent that can summarize tickets is safe, but an agent that can delete data or email customers requires controls.
2. Define guardrails as architectural controls across the loop.
3. Explain boundaries, ethics, and trustworthiness.
4. Show LangChain 1.0 middleware code for PII and human approval.
5. Discuss metrics, online/offline evals, and production monitoring.
6. Show a LangGraph interrupt example for approval gates.
7. Explain red teaming as scenario-based adversarial testing of tools, memory, orchestration, and multi-agent handoffs.
8. Close by connecting safety work to enterprise adoption in Chapter 6.

## Open Questions or Conflicts

- No blocker. Official LangChain and LangGraph docs verify the main APIs needed for Chapter 4.
- Avoid claiming that guardrails make an agent "safe" in an absolute sense. They reduce specific risks and must be tested.
- Avoid using `InMemorySaver` as a production recommendation. In chapter code, explicitly mark it as illustrative for development.
