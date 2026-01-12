---
name: oat
description: Meta-skill for managing Agent Skills and reference documents. Use this to create/update skills or store notes, knowledge, and other reference materials based on the current conversation context.
---

# Oat - Skill & Knowledge Manager

This skill allows you to create and update agent skills and reference documents within the repository where this skill is used.

## Skills

Executable instructions that define agent behavior for specific tasks.

### Create a New Skill
```
oat - create a new skill on <Topic or Description>
```

**Workflow:**
1.  **Analyze Request**: Understand the goal of the new skill.
2.  **Create Directory**: Create a new directory in `skills/<skill-name>`.
3.  **Draft Content**: Write the `SKILL.md` with appropriate frontmatter (`name`, `description`) based on the request.
4.  **Link (Proactive)**: Detect the user's agent ecosystem and propose the link automatically.
    -   *Detect*: Check for existing skills directories:
        - OpenCode: `~/.config/opencode/skill`
        - Claude Code: `~/.claude/skills`
        - Gemini CLI: `~/.gemini/skills`
    -   *Action*: If exactly one exists, propose that path and ask for confirmation to run the link. If multiple exist, ask which to use.
    -   *Command*: `ln -s "$(pwd)/skills/<skill-name>" <target_skills_directory>/<skill-name>`
    -   *Safety*: **Always** ask for explicit permission before executing the link command.

### Update an Existing Skill
```
oat - update <skill-name> skill on <Topic or Context>
```

**Workflow:**
1.  **Verify Existence**: Check if `skills/<skill-name>` exists. If not, ask the user for clarification.
2.  **Read Definition**: Read the current `skills/<skill-name>/SKILL.md`.
3.  **Update**: Modify the `SKILL.md` to incorporate the new logic/instructions derived from the current conversation or specific request.
4.  **Supporting Files**: Add scripts, templates, or documentation inside the skill folder as needed.

### Skill Folder Structure
```
skills/<skill-name>/
├── SKILL.md           # Required: skill definition with frontmatter
├── scripts/           # Optional: helper scripts
│   └── build.sh
├── templates/         # Optional: template files
│   └── config.yaml
└── docs/              # Optional: skill-specific documentation
    └── usage.md
```

### Skill Rules
-   Always store skills in the `skills/` directory of the repository.
-   Skills require a `SKILL.md` with frontmatter (`name`, `description`).
-   Supporting files (scripts, templates, references) go inside the skill folder alongside `SKILL.md`.
-   When linking, always use absolute paths for the source.
-   Ensure the target skills directory exists before linking.

---

## Reference Documents

Standalone markdown files that capture notes, knowledge, tips, learnings, or any reference information discovered during conversations.

### Create a New Reference Document
```
oat - create a <type> on <Topic or Description>
```

Where `<type>` can be: `note`, `knowledge`, `reference`, `tip`, `cheatsheet`, etc.

**Examples:**
- `oat - create a note on environment variables for non-interactive shells`
- `oat - create a knowledge doc on git rebase strategies`
- `oat - create a cheatsheet on Docker commands`

**Workflow:**
1.  **Analyze Request**: Understand what should be captured.
2.  **Determine Location**: Store the document in a sensible location within the repository (e.g., `docs/`, `notes/`, or the repository root). Ask the user if unclear.
3.  **Create File**: Create a new markdown file with a kebab-case filename (e.g., `non-interactive-environment-variables.md`).
4.  **Draft Content**: Write the document with:
    -   A clear `# Title` at the top.
    -   Organized sections explaining the concept.
    -   Code examples where applicable.
    -   A TL;DR section for quick reference (optional but recommended).

### Update an Existing Reference Document
```
oat - update <document-name> on <Topic or Context>
```

**Workflow:**
1.  **Locate Document**: Search for the document in the repository. If not found, ask the user for clarification.
2.  **Read Content**: Read the current document.
3.  **Update**: Modify the document to incorporate new information from the current conversation.

### Reference Document Rules
-   Store in a sensible location (e.g., `docs/`, `notes/`, or as the user specifies).
-   Use descriptive, kebab-case filenames.
-   Do NOT require frontmatter (unlike skills).
-   Do NOT need to be linked to agent configs—they serve as reference documentation.

---

## How to Decide: Skill vs Reference Document

| Create a **Skill** when... | Create a **Reference Document** when... |
|---------------------------|----------------------------------------|
| You need repeatable agent behavior | You want to capture information for future reference |
| It defines a workflow or procedure | It's a note, tip, or knowledge snippet |
| It should be invokable by the agent | It's documentation or a cheatsheet |
