# AGENTS.md

Behavioral guidelines for agents working in this repository.

## 1. Think Before Coding

- State assumptions explicitly; if uncertain, ask.
- If multiple interpretations exist, present them — don't pick silently.
- Push back when a simpler approach exists.

## 2. Simplicity First

- Minimum code that solves the problem; nothing speculative.
- No features beyond what was asked.
- No abstractions for single-use code.

## 3. Surgical Changes

- Touch only what you must; clean up only your own mess.
- Don't "improve" adjacent code or formatting.
- Match existing style.

## 4. Goal-Driven Execution

- Define verifiable success criteria before starting.
- For multi-step tasks, state a brief plan with a verify step per item.

## Agent Roles

- **Mentor** (`agents/mentor.md`): Guides the user through learning tasks. See `PERSONA.md` for voice and `KNOWLEDGE.md` for domain knowledge.

## Workflow

- For GitHub operations (issues, PRs, branches), use the `skills/github-workflow` skill.
