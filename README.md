# oat

A template for **Agent Skills** compatible with LLMs that adopt the Agent Skills standard (e.g., OpenCode, Claude Code, Gemini CLI).

## Installation

To use these skills, link them to your agent's configuration directory.

### OpenCode
OpenCode uses the `~/.config/opencode/skill` directory (note: singular `skill`).

```bash
# Link all skills
mkdir -p ~/.config/opencode/skill
ln -s "$(pwd)/skills/oat" ~/.config/opencode/skill/oat
```

### Claude Code
Claude Code uses the `~/.claude/skills` directory (note: plural `skills`).

```bash
# Link all skills
mkdir -p ~/.claude/skills
ln -s "$(pwd)/skills/oat" ~/.claude/skills/oat
```

### Gemini CLI
Gemini CLI uses the `~/.gemini/skills` directory.

```bash
# Link all skills
mkdir -p ~/.gemini/skills
ln -s "$(pwd)/skills/oat" ~/.gemini/skills/oat
```

## Available Skills

- **oat**: A meta-skill to create and manage other skills in this repository.