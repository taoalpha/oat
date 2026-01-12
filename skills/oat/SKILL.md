---
name: oat
description: Meta-skill for managing Agent Skills. Use this to create new skills or update existing ones based on the current conversation context, compatible with any LLM using the agent skills standard.
---

# Oat - Skill Manager

This skill allows you to create and update standard agent skills within this repository and automatically link them to your AI agent's configuration.

## Usage

### Create a New Skill
Create a new skill based on a topic or instructions.

```
/oat create a new skill on <Topic or Description>
```

**Workflow:**
1.  **Analyze Request**: Understand the goal of the new skill.
2.  **Create Directory**: Create a new directory in `skills/<skill-name>`.
3.  **Draft Content**: Write the `SKILL.md` with appropriate frontmatter (`name`, `description`) based on the request.
4.  **Link**: Prompt the user to link the new skill to their agent's skills directory.
    -   *Action*: Ask the user for the target directory path (e.g., `~/.config/opencode/skill` for OpenCode, `~/.claude/skills` for Claude Code) if not already known.
    -   *Command*: `ln -s "$(pwd)/skills/<skill-name>" <target_skills_directory>/<skill-name>`
    -   *Safety*: **Always** ask for explicit permission before executing the link command.

### Update an Existing Skill
Update an existing skill using the context of the current conversation (e.g., adding new capabilities, fixing bugs, improving documentation).

```
/oat update <skill-name> on <Topic or Context>
```

**Workflow:**
1.  **Verify Existence**: Check if `skills/<skill-name>` exists. If not, ask the user for clarification.
2.  **Read Definition**: Read the current `skills/<skill-name>/SKILL.md`.
3.  **Update**: Modify the `SKILL.md` to incorporate the new logic/instructions derived from the current conversation or specific request.
    -   Create additional files (scripts, references) in the skill folder if necessary to support the update.

## General Rules
-   Always store skills in the `skills/` directory of this repository (the same parent directory as this `oat` skill).
-   When linking, always use absolute paths for the source.
-   Ensure the target skills directory exists before linking.
