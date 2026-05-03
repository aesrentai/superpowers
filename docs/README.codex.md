# Superpowers for Codex CLI

Guide for using Superpowers with OpenAI Codex CLI via native skill and custom-agent discovery.

## Quick Install

Tell Codex:

```
Fetch and follow instructions from https://raw.githubusercontent.com/aesrentai/superpowers/refs/heads/main/.codex/INSTALL.md
```

## Manual Installation

### Prerequisites

- OpenAI Codex CLI
- Git

### Steps

1. Clone the repo:
   ```bash
   git clone https://github.com/aesrentai/superpowers.git ~/.codex/superpowers
   ```

2. Create the skills and agent symlinks:
   ```bash
   mkdir -p ~/.agents/skills ~/.codex/agents
   ln -s ~/.codex/superpowers/skills ~/.agents/skills/superpowers
   ln -s ~/.codex/superpowers/.codex/agents/code-reviewer.toml ~/.codex/agents/code-reviewer.toml
   ```

3. Restart Codex.

### Windows

Use a junction instead of a symlink (works without Developer Mode):

```powershell
New-Item -ItemType Directory -Force -Path "$env:USERPROFILE\.agents\skills"
New-Item -ItemType Directory -Force -Path "$env:USERPROFILE\.codex\agents"
cmd /c mklink /J "$env:USERPROFILE\.agents\skills\superpowers" "$env:USERPROFILE\.codex\superpowers\skills"
Copy-Item "$env:USERPROFILE\.codex\superpowers\.codex\agents\code-reviewer.toml" "$env:USERPROFILE\.codex\agents\code-reviewer.toml" -Force
```

## How It Works

Codex has native skill discovery — it scans `~/.agents/skills/` at startup, parses SKILL.md frontmatter, and loads skills on demand. Superpowers skills are made visible through a single symlink:

```
~/.agents/skills/superpowers/ → ~/.codex/superpowers/skills/
```

Codex also loads custom agents from `~/.codex/agents/`. Superpowers installs a `code-reviewer` agent there so review requests can use a real Codex custom agent instead of only a prompt template.

The `using-superpowers` skill is discovered automatically and enforces skill usage discipline.

## Usage

Skills are discovered automatically. Codex activates them when:
- You mention a skill by name (e.g., "use brainstorming")
- The task matches a skill's description
- The `using-superpowers` skill directs Codex to use one

### Personal Skills

Create your own skills in `~/.agents/skills/`:

```bash
mkdir -p ~/.agents/skills/my-skill
```

Create `~/.agents/skills/my-skill/SKILL.md`:

```markdown
---
name: my-skill
description: Use when [condition] - [what it does]
---

# My Skill

[Your skill content here]
```

The `description` field is how Codex decides when to activate a skill automatically — write it as a clear trigger condition.

## Updating

```bash
cd ~/.codex/superpowers && git pull
```

Skills and the custom agent update instantly through the symlinks.

## Uninstalling

```bash
rm ~/.agents/skills/superpowers
rm ~/.codex/agents/code-reviewer.toml
```

**Windows (PowerShell):**
```powershell
Remove-Item "$env:USERPROFILE\.agents\skills\superpowers"
Remove-Item "$env:USERPROFILE\.codex\agents\code-reviewer.toml"
```

Optionally delete the clone: `rm -rf ~/.codex/superpowers` (Windows: `Remove-Item -Recurse -Force "$env:USERPROFILE\.codex\superpowers"`).

## Troubleshooting

### Skills not showing up

1. Verify the symlink: `ls -la ~/.agents/skills/superpowers`
2. Check skills exist: `ls ~/.codex/superpowers/skills`
3. Restart Codex — skills are discovered at startup

### Code reviewer agent not showing up

1. Verify the symlink: `ls -la ~/.codex/agents/code-reviewer.toml`
2. Check the source exists: `ls ~/.codex/superpowers/.codex/agents/code-reviewer.toml`
3. Restart Codex — custom agents are discovered at startup

### Windows junction issues

Junctions normally work without special permissions. If creation fails, try running PowerShell as administrator.

## Getting Help

- Report issues: https://github.com/obra/superpowers/issues
- Main documentation: https://github.com/obra/superpowers
