# Installing Superpowers for Codex

Enable Superpowers skills and the Codex code-reviewer agent in Codex CLI. Clone once, then symlink the reusable pieces into Codex's discovery paths.

## Prerequisites

- Git

## Installation

1. **Clone Cody's Superpowers fork:**
   ```bash
   git clone https://github.com/aesrentai/superpowers.git ~/.codex/superpowers
   ```

2. **Create the skills and agent symlinks:**
   ```bash
   mkdir -p ~/.agents/skills ~/.codex/agents
   ln -s ~/.codex/superpowers/skills ~/.agents/skills/superpowers
   ln -s ~/.codex/superpowers/.codex/agents/code-reviewer.toml ~/.codex/agents/code-reviewer.toml
   ```

   **Windows (PowerShell):**
   ```powershell
   New-Item -ItemType Directory -Force -Path "$env:USERPROFILE\.agents\skills"
   New-Item -ItemType Directory -Force -Path "$env:USERPROFILE\.codex\agents"
   cmd /c mklink /J "$env:USERPROFILE\.agents\skills\superpowers" "$env:USERPROFILE\.codex\superpowers\skills"
   Copy-Item "$env:USERPROFILE\.codex\superpowers\.codex\agents\code-reviewer.toml" "$env:USERPROFILE\.codex\agents\code-reviewer.toml" -Force
   ```

3. **Restart Codex** (quit and relaunch the CLI) to discover the skills and agent.

## Migrating from old bootstrap

If you installed superpowers before native skill discovery, you need to:

1. **Update the repo:**
   ```bash
   cd ~/.codex/superpowers && git pull
   ```

2. **Create the skills and agent symlinks** (step 2 above) — this is the new discovery mechanism.

3. **Remove the old bootstrap block** from `~/.codex/AGENTS.md` — any block referencing `superpowers-codex bootstrap` is no longer needed.

4. **Restart Codex.**

## Verify

```bash
ls -la ~/.agents/skills/superpowers
ls -la ~/.codex/agents/code-reviewer.toml
```

You should see a symlink (or junction on Windows) pointing to your Superpowers skills directory and a Codex `code-reviewer` custom agent.

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

Optionally delete the clone: `rm -rf ~/.codex/superpowers`.
