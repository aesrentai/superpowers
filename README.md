# Superpowers

> Note: This branch has been modified by Cody to fit his Codex-first workflow, token budget, and programming style. Some wording and workflow choices intentionally differ from upstream Superpowers.

Superpowers is a complete software development methodology for your coding agents, built on top of a set of composable skills and some initial instructions that make sure your agent uses them.

## How it works

It starts from the moment you fire up your coding agent. As soon as it sees that you're building something non-trivial, it *doesn't* just jump into trying to write code. Instead, it steps back and asks you what you're really trying to do.

Straightforward one-shot work stays lightweight: a small new project whose scope is not obviously large, or a bugfix that preserves the existing spec/API/behavior contract, can be implemented directly, verified, and reported without dragging in the full workflow.

Once it's teased a spec out of the conversation, it shows it to you in chunks short enough to actually read and digest. When there is a clearly right approach, the agent should recommend it directly; it should only present multiple options when they are real options with meaningful trade-offs.

After you've signed off on the design, your agent puts together an implementation plan that's clear enough for an enthusiastic junior engineer with poor taste, no judgement, no project context, and an aversion to testing to follow. The plan should capture interfaces, file structure, high-level implementation details, files to touch, and verification steps without requiring complete code for every change. It emphasizes real behavior checks, YAGNI (You Aren't Gonna Need It), and DRY.

Next up, once you say "go", it launches a *subagent-driven-development* process, having agents work through each engineering task, self-review, run the relevant verification, and get a focused spec-compliance review before continuing. It avoids extra style-review loops and commit spam; commits happen at meaningful checkpoints like a confirmed major feature or a bugfix on an existing feature.

Testing focuses on behavior that matters: user-facing functionality, exported interfaces, and regression tests for real bugs. Internal implementation details do not need direct tests unless there is a specific reason, such as a tricky algorithm, performance sensitivity, or no practical way to verify the behavior through an external boundary.

Code review is reserved for substantial checkpoints: after a major feature, before merge, before opening a PR, or when risk justifies the extra pass. Reviewers should focus on concrete defects, missing behavior, real-interface test gaps, type-safety issues, and obvious bad choices rather than open-ended style or architecture commentary.

There's a bunch more to it, but that's the core of the system. And because the skills trigger automatically, you don't need to do anything special. Your coding agent just has Superpowers.


## Sponsorship

If Superpowers has helped you do stuff that makes money and you are so inclined, I'd greatly appreciate it if you'd consider [sponsoring my opensource work](https://github.com/sponsors/obra).

Thanks! 

- Jesse


## Installation

**Note:** Installation differs by platform. 

### OpenAI Codex CLI

For this Codex-first fork, use the installer so both the skills and the Codex `code-reviewer` custom agent are installed:

```text
Fetch and follow instructions from https://raw.githubusercontent.com/aesrentai/superpowers/refs/heads/main/.codex/INSTALL.md
```

The Codex plugin directory install is still available upstream, but the manual installer is the path that wires this fork's custom agent into Codex CLI.

### Claude Code Official Marketplace

Superpowers is available via the [official Claude plugin marketplace](https://claude.com/plugins/superpowers)

Install the plugin from Anthropic's official marketplace:

```bash
/plugin install superpowers@claude-plugins-official
```

### Claude Code (Superpowers Marketplace)

The Superpowers marketplace provides Superpowers and some other related plugins for Claude Code.

In Claude Code, register the marketplace first:

```bash
/plugin marketplace add obra/superpowers-marketplace
```

Then install the plugin from this marketplace:

```bash
/plugin install superpowers@superpowers-marketplace
```

### Cursor (via Plugin Marketplace)

In Cursor Agent chat, install from marketplace:

```text
/add-plugin superpowers
```

or search for "superpowers" in the plugin marketplace.

### OpenCode

Tell OpenCode:

```
Fetch and follow instructions from https://raw.githubusercontent.com/obra/superpowers/refs/heads/main/.opencode/INSTALL.md
```

**Detailed docs:** [docs/README.opencode.md](docs/README.opencode.md)

### GitHub Copilot CLI

```bash
copilot plugin marketplace add obra/superpowers-marketplace
copilot plugin install superpowers@superpowers-marketplace
```

### Gemini CLI

```bash
gemini extensions install https://github.com/obra/superpowers
```

To update:

```bash
gemini extensions update superpowers
```

## The Basic Workflow

For one-shot work, the agent skips the full workflow and implements directly. For larger, ambiguous, or spec-changing work, the workflow is:

1. **brainstorming** - Activates before creative or non-trivial work, not for straightforward narrowly scoped changes. Refines rough ideas through questions, thinks through the approach, presents real alternatives only when they exist, and saves a design document.

2. **using-git-worktrees** - Activates after design approval. Creates isolated workspace on new branch, runs project setup, verifies clean test baseline.

3. **writing-plans** - Activates with approved design. Breaks work into bite-sized tasks with exact file paths, interfaces, file structure, high-level implementation details, and verification steps.

4. **subagent-driven-development** or **executing-plans** - Activates with plan. Dispatches fresh subagent per task with spec compliance review, or executes in batches with human checkpoints. Commits are for meaningful checkpoints, not every small task.

5. **test-driven-development** - Activates for external behavior changes. Enforces RED-GREEN-REFACTOR for user-visible behavior, exported interfaces, and bug regressions, with a strong preference for tests or verification through the real boundary.

6. **requesting-code-review** - Activates at substantial review points. Reviews the diff against requirements and nearby code, reports concrete issues by severity, and avoids style-only or speculative recommendations.

7. **finishing-a-development-branch** - Activates when tasks complete. Verifies tests, presents options (merge/PR/keep/discard), cleans up worktree.

**The agent checks for relevant skills before any task.** Mandatory workflows, not suggestions.

## What's Inside

### Skills Library

**Testing**
- **test-driven-development** - RED-GREEN-REFACTOR cycle for external behavior and bug regressions (includes testing anti-patterns reference)

**Debugging**
- **systematic-debugging** - 4-phase root cause process (includes root-cause-tracing, defense-in-depth, condition-based-waiting techniques)
- **verification-before-completion** - Ensure it's actually fixed

**Collaboration** 
- **brainstorming** - Socratic design refinement for non-trivial work
- **writing-plans** - Implementation plans with interfaces, file structure, high-level details, and verification
- **executing-plans** - Batch execution with checkpoints
- **dispatching-parallel-agents** - Concurrent subagent workflows
- **requesting-code-review** - Bounded review at substantial checkpoints
- **receiving-code-review** - Responding to feedback
- **using-git-worktrees** - Parallel development branches
- **finishing-a-development-branch** - Merge/PR decision workflow
- **subagent-driven-development** - Fast iteration with spec compliance review and commit discipline

**Meta**
- **writing-skills** - Create new skills following best practices (includes testing methodology)
- **using-superpowers** - Introduction to the skills system, including the one-shot exception

## Philosophy

- **Behavior-first testing** - Test user-facing behavior, exported interfaces, and real regressions; test internals only when risk justifies it
- **Systematic over ad-hoc** - Process over guessing
- **Complexity reduction** - Simplicity as primary goal
- **Evidence over claims** - Verify before declaring success
- **Review where it matters** - Use review for substantial checkpoints and concrete risk, not token-burning style passes

Read [the original release announcement](https://blog.fsck.com/2025/10/09/superpowers/).

## Contributing

The general contribution process for Superpowers is below. Keep in mind that we don't generally accept contributions of new skills and that any updates to skills must work across all of the coding agents we support.

1. Fork the repository
2. Switch to the 'dev' branch
3. Create a branch for your work
4. Follow the `writing-skills` skill for creating and testing new and modified skills
5. Submit a PR, being sure to fill in the pull request template.

See `skills/writing-skills/SKILL.md` for the complete guide.

## Updating

Superpowers updates are somewhat coding-agent dependent, but are often automatic.

## License

MIT License - see LICENSE file for details

## Community

Superpowers is built by [Jesse Vincent](https://blog.fsck.com) and the rest of the folks at [Prime Radiant](https://primeradiant.com).

- **Discord**: [Join us](https://discord.gg/35wsABTejz) for community support, questions, and sharing what you're building with Superpowers
- **Issues**: https://github.com/obra/superpowers/issues
- **Release announcements**: [Sign up](https://primeradiant.com/superpowers/) to get notified about new versions
