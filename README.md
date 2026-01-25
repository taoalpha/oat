# oat

A template for **Agent Skills** compatible with LLMs that adopt the Agent Skills standard (e.g., OpenCode, Claude Code, Gemini CLI).

## Installation

To use these skills, link them to your agent's configuration directory.

### OpenCode
OpenCode uses the `~/.config/opencode/skill` directory (note: singular `skill`).

```bash
# Link all skills
mkdir -p ~/.config/opencode
ln -s "$(pwd)/skills" ~/.config/opencode/skill
```

### Claude Code
Claude Code uses the `~/.claude/skills` directory (note: plural `skills`).

```bash
# Link all skills
mkdir -p ~/.claude
ln -s "$(pwd)/skills" ~/.claude/skills
```

### Gemini CLI
Gemini CLI uses the `~/.gemini/skills` directory.

```bash
# Link all skills
mkdir -p ~/.gemini
ln -s "$(pwd)/skills" ~/.gemini/skills
```

## Available Skills

- **oat**: A meta-skill to create and manage other skills in this repository.
