# AI Agents For Professionals Style Guide

## Purpose

This guide keeps every chapter of *AI Agents For Professionals* consistent in voice, structure, terminology, examples, and code treatment. Treat it as binding for all chapter drafts.

## Audience

Write for working professionals: software engineers, architects, technical leaders, product-minded AI practitioners, and enterprise technology stakeholders who need to evaluate, design, build, or govern agentic AI systems. Assume strong professional judgment and general software literacy, but do not assume prior hands-on experience with LangGraph, LangChain, or agent architecture.

## Voice and Register

- Use a professional, energetic voice that communicates why agentic systems matter without hype.
- Keep the register between academic and industry technical writing: precise, accessible, and practical.
- Prefer clear declarative sentences. Vary sentence length, but avoid casual slang, sales language, and excessive metaphor.
- Use first-person plural sparingly for shared learning moments: "we will see," "we can model." Avoid overusing "you" as a directive.
- Define technical terms on first use, then reuse the same term consistently.

## Chapter Shape

Each chapter follows this sequence:

1. Chapter title matching the table of contents.
2. A compelling opening hook: a scenario, real problem, provocative question, or vivid analogy.
3. A clear motivation section that explains why the concept matters and what breaks without it.
4. Major sections in the same order and names as `TableOFContents1.md`.
5. Each major subsection begins with motivation before mechanism: first explain why the idea matters, then explain how it works.
6. Professional learning progression: practical motivation -> concept -> mechanism -> code or example -> operational implications.
7. At least one current real-world example grounded in research.
8. Mermaid diagrams where they clarify flow, architecture, feedback loops, state graphs, decision points, or system boundaries.
9. A short closing recap with a bridge to the next chapter when useful.

## Terminology

Use these terms consistently:

- "agent" for a system that perceives, reasons or selects actions, and acts toward a goal.
- "AI agent" for an agent whose decision-making uses an AI model.
- "agentic AI" for systems that combine goals, context, planning, tool use, feedback, and action over time.
- "tool" for an external capability an agent can call, such as search, a database query, or an API action.
- "state" for the structured information carried through an agent workflow.
- "node" for a LangGraph step that reads state and returns updates.
- "edge" for the transition between LangGraph nodes.
- "graph" for the composed LangGraph workflow.
- "guardrail" for a control that constrains, checks, or monitors agent behavior.

Do not silently swap in synonyms such as "bot," "assistant," "actor," or "module" unless contrasting concepts explicitly.

## Code Standards

- Use Python unless the outline requires another language.
- Target LangGraph 1.0 and LangChain 1.0.
- Verify import paths, class names, and method signatures against current official documentation before including code.
- Keep snippets illustrative rather than complete applications. Show only the relevant code needed to teach the concept.
- Every code block must be preceded by one sentence explaining what it demonstrates.
- Every code block must include short inline comments that explain why key choices are made.
- Every code block must be followed by a brief explanation of the key lines and how they connect to the concept.
- If an API cannot be verified from current official documentation, do not guess. State the uncertainty in the research notes and avoid presenting it as authoritative chapter code.

## Diagrams

- Use fenced `mermaid` blocks.
- Keep diagrams small enough to be read in Markdown.
- Label nodes with plain English.
- Use diagrams for loops, graphs, control flow, multi-agent coordination, safety boundaries, and enterprise governance flows.

## Cross-References

Use this exact format for cross-references:

> As introduced in Chapter N, "Section Title", ...

When referring forward:

> This is explored in depth in Chapter N, "Section Title".

Reference chapter numbers and section titles exactly as they appear in `TableOFContents1.md`. Do not create Markdown links until the target file exists and the heading text has been finalized.

## Research Notes

Each chapter research file must include:

- Research date.
- Chapter title.
- Source list with URLs.
- Version-specific notes for LangGraph 1.0 and LangChain 1.0 when code or API behavior is involved.
- Real-world examples considered for the chapter.
- Conflicts or uncertainties, resolved in favor of the most recent official documentation.

## Definition of Done

Before a chapter is marked `done`, confirm:

- Research notes are saved.
- Opening hook and motivation are present.
- Every major subsection uses motivation before mechanism.
- Explanations progress from intuition to mechanism to example.
- At least one researched real-world example is included.
- Code snippets follow the code standards above.
- Mermaid diagrams are included wherever useful.
- Cross-references use the exact guide format.
- The closing recap is present.
- `book/PROGRESS.md` is updated.
