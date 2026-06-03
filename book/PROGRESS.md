# AI Agents For Professionals (Langflow Low-Code Edition) Progress

Status values: `pending`, `researched`, `drafted`, `done`.

The book is being rewritten from a LangChain/LangGraph (code-first) basis to a **Langflow (low-code, visual)** basis. Shared research and conventions:

- Style guide: `book/STYLE_GUIDE.md`
- Shared Langflow research: `book/research/langflow-rewrite-research.md`

| Chapter | Title | Chapter File | Status |
|---|---|---|---|
| 01 | Understanding Agents: The Core Concept | `book/chapters/chapter-01-understanding-agents-the-core-concept.md` | `done` |
| 02 | From Agents to Agentic AI | `book/chapters/chapter-02-from-agents-to-agentic-ai.md` | `done` |
| 03 | Multi-Agent Systems and Collaboration | `book/chapters/chapter-03-multi-agent-systems-and-collaboration.md` | `done` |
| 04 | Ensuring Safe and Responsible Agentic AI | `book/chapters/chapter-04-ensuring-safe-and-responsible-agentic-ai.md` | `done` |
| 05 | Transitioning from Traditional Engineering to Agentic AI | `book/chapters/chapter-05-transitioning-from-traditional-engineering-to-agentic-ai.md` | `done` |
| 06 | Enterprise Adoption of Agentic AI | `book/chapters/chapter-06-enterprise-adoption-of-agentic-ai.md` | `done` |
| 07 | The Future of Agentic AI and Your Role | `book/chapters/chapter-07-the-future-of-agentic-ai-and-your-role.md` | `done` |
| 08 | Personal Agents: Always-On Assistants You Run Yourself | `book/chapters/chapter-08-personal-agents-openclaw-and-hermes.md` | `done` |

## Current Status

Langflow low-code rewrite complete. All seven chapters build agents as Langflow flows (Agent component, Tool Mode tools, MCP server/client, Playground, Policies, Traces) instead of hand-written LangChain/LangGraph code. Verification passed: no stray LangChain/LangGraph/LangSmith references, structure and cross-references preserved, no linter errors.

## Open items for an editing pass

- Langflow features described at the capability level (Policies, Traces/Inspection Panel, Langflow 1.9 Assistant / Flow DevOps Toolkit / V2 APIs) should be re-verified against current 1.9.x docs before publication; exact UI labels and steps can shift.
- Verify exact MCP endpoint path (`/api/v1/mcp/project/PROJECT_ID/streamable`), port labels ("Tools"/"Toolset"), and the If-Else router's ports/operators against 1.9.x docs.
