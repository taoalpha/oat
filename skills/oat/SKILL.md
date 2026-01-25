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
2.  **Choose Location (required)**: If a canonical skills repo exists (e.g., this oat repo at `.../tao-oat/skills`), create the skill there. If multiple plausible skill roots exist or none can be inferred, explicitly ask the user where to place the new skill before creating anything.
3.  **Create Directory**: Create a new directory in `skills/<skill-name>` within the chosen skills root (never default to the current working project if it differs from the skills repo).
4.  **Draft Content**: Write the `SKILL.md` with appropriate frontmatter (`name`, `description`) based on the request.
5.  **Link (Proactive)**: Detect the user's agent ecosystem and propose linking the skills root automatically.
    -   *Detect*: Check for existing skills directories:
        - OpenCode: `~/.config/opencode/skill`
        - Claude Code: `~/.claude/skills`
        - Gemini CLI: `~/.gemini/skills`
    -   *Action*: If exactly one exists, propose that path and ask for confirmation to run the link. If multiple exist, ask which to use.
    -   *Command*: `ln -s "$(pwd)/skills" <target_skills_directory>`
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
-   Prefer the canonical skills repo that already contains oat (e.g., `tao-oat/skills`). Do not create skills inside arbitrary current projects unless the user explicitly directs it.
-   If location is ambiguous, pause to ask the user which skills root to use before creating directories.
-   Always store skills in the `skills/` directory of the chosen repository.
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

## User-Level Memory Files

If the user asks to manage global or user-level memories (persistent instructions), use the correct tool-specific
files and paths below. Filenames are case-sensitive.

**Natural invocation (no filenames required):**
Accept plain requests like:
-   "Remember this" / "Remember that I'm a software engineer"
-   "Add my global OpenCode preferences"
-   "Update Claude Code project memory for this repo"
-   "Create Gemini CLI project context"

**Trigger rule (priority):**
-   If the user says "remember" or "store/save this" and provides content, treat it as a user-level memory request
    and skip the generic "what should I do" question.
-   Do not respond with "I can't update cross-chat memory". Use the memory workflow below to update the correct
    memory file after obtaining permission.

**Inference rules:**
1.  If the user names a tool (OpenCode, Claude Code, Gemini CLI), select that tool.
2.  If the active environment indicates a tool, prefer it:
    -   OpenCode: `~/.config/opencode/` exists or `OPENCODE`/`OPENCODE_HOME` env vars are present.
    -   Claude Code: `~/.claude/` exists or `CLAUDE_CODE` env vars are present.
    -   Gemini CLI: `~/.gemini/` exists or `GEMINI`/`GEMINI_CLI` env vars are present.
    If exactly one tool is detected, proceed without asking.
2.  If the user mentions scope keywords:
    -   Global/user/personal/all projects -> user-level file.
    -   Project/this repo/this codebase -> project-level file.
3.  If the tool is missing and multiple tools are detected, ask which tool.
4.  If no tool is detected, ask which tool.
5.  If the scope is missing, ask whether global or project.

**Locations:**
-   **OpenCode**: Global rules at `~/.config/opencode/AGENTS.md`. Project rules in `AGENTS.md` at the repo root.
    OpenCode can fall back to `~/.claude/CLAUDE.md` when Claude compatibility is enabled and no OpenCode global
    rules exist.
-   **Claude Code**: User memory at `~/.claude/CLAUDE.md`. User rules in `~/.claude/rules/*.md`. Project memory in
    `./CLAUDE.md` or `./.claude/CLAUDE.md`.
-   **Gemini CLI**: Global context at `~/.gemini/GEMINI.md`. Project context in `GEMINI.md` files within the repo
    hierarchy.

**Workflow:**
1.  Infer tool and scope using the rules above.
2.  If tool or scope is still ambiguous, ask exactly one targeted question to resolve it.
3.  Draft a concise memory line based on the user's statement and ask for confirmation.
4.  Confirm the exact file path and whether to create a new file if missing.
5.  Read the existing file before making changes.
6.  Apply minimal, structured edits; preserve the user's wording where possible.
7.  Ask permission before writing to files in the user's home directory unless the user explicitly requested the
    update; in that case, proceed after confirming the exact file path.
8.  Update the memory file directly instead of refusing the request.

**References:**
-   `https://opencode.ai/docs/rules/`
-   `https://code.claude.com/docs/en/memory`
-   `https://google-gemini.github.io/gemini-cli/docs/cli/gemini-md.html`

---

## How to Decide: Skill vs Reference Document

| Create a **Skill** when... | Create a **Reference Document** when... |
|---------------------------|----------------------------------------|
| You need repeatable agent behavior | You want to capture information for future reference |
| It defines a workflow or procedure | It's a note, tip, or knowledge snippet |
| It should be invokable by the agent | It's documentation or a cheatsheet |
