# Chapter 8 Research — Personal Agents (OpenClaw and Hermes)

**Research date:** 2026-06-02
**Chapter:** Personal Agents: Always-On Assistants You Run Yourself

## Sources

- OpenClaw repo — https://github.com/openclaw/openclaw/
- OpenClaw docs (Gateway, configuration) — https://docs.openclaw.ai/
- "OpenClaw: complete guide" (Lenny's Newsletter) — https://www.lennysnewsletter.com/p/openclaw-the-complete-guide-to-building
- "OpenClaw Changed Her Life" — https://dougvos.com/openclaw-changed-her-life/
- "7 Practical OpenClaw Use Cases" (KDnuggets) — https://www.kdnuggets.com/7-practical-openclaw-use-cases-you-should-know
- OpenClaw use case (overnight mini-app builder) — https://openclawai.io/use-cases/overnight-mini-app-builder
- openclaw-notebook (community stack) — https://github.com/akshay93aditya/openclaw-notebook
- Hermes repo — https://github.com/NousResearch/hermes-agent
- Hermes site — https://hermes-agent.org/
- hermes_stack (community skills) — https://github.com/kongaharsha/hermes_stack
- "Hermes Agent vs OpenClaw — Full Breakdown" (Turing Post) — https://www.turingpost.com/p/hermes
- "Hermes Agent in the Wild: AI Ops Employee" (dev.to) — https://dev.to/samarth28/hermes-agent-in-the-wild-how-i-turned-it-into-an-ai-ops-employee-2l85
- "Hermes vs OpenClaw & GoClaw" (dev.to) — https://dev.to/truongpx396/hermes-agent-the-self-improving-agent-framework-and-how-it-compares-to-openclaw-goclaw-22mc

## Key facts used

### OpenClaw
- Open-source personal AI assistant, run on your own devices, any OS/platform. TypeScript/Node. Permissive (MIT-style) license.
- Local-first Gateway = control plane for sessions, channels, tools, events. "Gateway is just the control plane; the product is the assistant."
- Multi-channel: WhatsApp, Telegram, Slack, Discord, Signal, iMessage, and many more.
- Multi-agent routing: inbound channels/contacts routed to isolated agents (workspaces + per-agent sessions). Users run specialized agents (e.g., a sales agent doing CRM sweeps; a family-manager agent parsing schedules).
- Heartbeat: scheduled cron-like checks wake the agent to review to-dos and monitor calendars without prompting.
- Skills + capabilities file; onboarding wizard; first-class tools (browser, canvas, cron, chat-platform actions).
- Runs locally / overnight; can control your computer.
- GoClaw = Go reimplementation for multi-tenant, production/enterprise use (stronger isolation).

### Hermes (Nous Research, released early 2026)
- Open-source self-improving agent. Python + TS. Permissive license. "The agent that grows with you."
- Built-in closed learning loop: creates skills from experience, improves them in use, persists knowledge, searches past sessions, builds a user model across sessions.
- Skills follow the agentskills.io open standard (shareable/portable).
- Model-agnostic (hosted portal, aggregator, or own endpoint; switch with one command).
- Runs anywhere: $5 VPS to GPU cluster; serverless backends hibernate when idle.
- Multi-platform gateway: Telegram, Discord, Slack, WhatsApp, Signal, CLI; voice transcription; cross-platform continuity.
- Subagents for parallel work; built-in cron scheduler; doubles as a research platform (trajectory generation, RL).

### Documented use cases (both)
- Daily/weekly briefings; calendar coordination and meeting prep; inbox/admin triage and reply drafting; second brain (notes/recall); dev & ops assistant (code review, standups, monitoring, night-shift on-call); niche research briefs; goal-driven autonomous tasks (incl. overnight app building); hands-on errands via reviewable skills (airline check-in, laundry booking).

## Langflow connection (book's low-code lens)
- Personal-agent capabilities map to Langflow building blocks: Agent component (loop), Tool Mode (tools), MCP Tools (external systems), session memory (`session_id`), agent-as-a-tool (specialist sub-agents), `/run` API called by an external scheduler (the "heartbeat"), Policies (guardrails), global variables/Credentials (secrets).
- Pragmatic hybrid: dedicated runtime owns the always-on multi-channel front end; governed Langflow flows exposed to it over MCP.

## Uncertainties / cautions
- Fast-moving space; star counts, exact feature lists, and version specifics shift. Chapter describes capabilities and patterns, not precise version/config steps.
- Licenses noted as permissive/MIT-style per sources (GoClaw noted elsewhere as non-commercial CC BY-NC); verify exact license before any redistribution claims.
- No OpenClaw/Hermes screenshots were embedded (third-party/licensing); chapter uses Mermaid diagrams instead.
