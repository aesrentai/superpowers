# Codex Notes

Codex is a first-class Superpowers environment. Skills load natively; when a Superpowers skill applies, use Codex's native skill, planning, file, shell, and subagent tools.

## Agents

Codex supports built-in agents (`default`, `worker`, `explorer`) and custom agents from `~/.codex/agents/*.toml` or `.codex/agents/*.toml`. Installed plugins may also provide agents.

When a Superpowers skill names an agent such as `code-reviewer`, use the matching Codex agent if available. If it is not available, dispatch the appropriate built-in role with the referenced prompt template.

## Prompt Templates

Some Superpowers files are prompt templates rather than standalone skills:

- `skills/subagent-driven-development/implementer-prompt.md`
- `skills/subagent-driven-development/spec-reviewer-prompt.md`
- `skills/requesting-code-review/code-reviewer.md`
- `skills/brainstorming/spec-document-reviewer-prompt.md`
- `skills/writing-plans/plan-document-reviewer-prompt.md`

Fill placeholders and dispatch the prompt with the appropriate Codex agent role.
