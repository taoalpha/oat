---
name: oat
description: Meta-skill for managing Agent Skills and reference documents. Use this to create/update skills or store notes, knowledge, and other reference materials based on the current conversation context.
---

# Oat - Skill & Knowledge Manager

This skill creates and updates agent skills and reference documents inside the repo where this skill lives.

## Expected Usage (Template Repo)

1. Create a new private git repo using oat as the template.
2. Keep `skills/`, `knowledge/`, `notes/`, `scripts/`, and `me.md` at the repo root.
3. Create new skills alongside the oat skill under `skills/`.

## Quick Commands

```
/oat create a new skill on <Topic or Description>
/oat update <skill-name> skill on <Topic or Context>
/oat create a <type> on <Topic or Description>
/oat update <document-name> on <Topic or Context>
```

## Skill Creation

**Workflow**
1. Analyze the request and intended behavior.
2. Choose location (required): use the canonical skills repo that already contains oat (e.g., `.../tao-oat/skills`).
   If multiple plausible skill roots exist or none can be inferred, ask where to place it before creating files.
3. Create directory: `skills/<skill-name>`.
4. Draft `SKILL.md` with frontmatter (`name`, `description`).
5. Link skills root (proactive): detect the user's agent ecosystem and propose linking.
   - Detect existing skill dirs:
     - OpenCode: `~/.config/opencode/skill`
     - Claude Code: `~/.claude/skills`
     - Gemini CLI: `~/.gemini/skills`
   - If exactly one exists, propose it and ask to run the link.
   - If multiple exist, ask which to use.
   - Command: `ln -s "$(pwd)/skills" <target_skills_directory>`
   - Always ask explicit permission before running the link command.

**Skill Folder Structure**
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

**Skill Rules**
- Prefer the canonical skills repo that already contains oat; do not create skills inside arbitrary projects.
- If location is ambiguous, ask which skills root to use.
- Keep names kebab-case and match directory names.
- Supporting files live alongside `SKILL.md` inside the skill folder.
- Use absolute paths when executing link commands.
- Ensure the target skills directory exists before linking.

## Skill Update

**Workflow**
1. Verify `skills/<skill-name>` exists; if not, ask.
2. Read the current `skills/<skill-name>/SKILL.md`.
3. Update instructions to reflect the new request or conversation context.
4. Add supporting files (scripts/templates/docs) only if needed.

## Reference Documents

Standalone markdown docs for notes, knowledge, tips, or reference info.

**Types**: `note`, `knowledge`, `reference`, `tip`, `cheatsheet`, etc.

**Workflow**
1. Analyze what should be captured.
2. Pick a sensible location (`docs/`, `notes/`, or repo root). Ask if unclear.
3. Create a kebab-case filename (e.g., `non-interactive-environment-variables.md`).
4. Draft content:
   - `# Title` at the top.
   - Organized sections.
   - Examples where useful.
   - Optional TL;DR.

**Reference Rules**
- No frontmatter required.
- No linking to agent configs needed.
- Keep scope narrow and focused.

## Memories (User-Level)

Use this section when the user asks to store global/user-level memories.

**Commands**
- `/oat remember <text>` to add or update a memory line in `me.md`.
- `/oat forget <text>` to remove a memory line from `me.md`.
- `/oat list memories` to list all content in `me.md`.

**Source of Truth**
- `me.md` is the canonical user-level memory file.
- Always update `me.md` for user-level memory changes.
- Assume user-level memory files are symlinked to `me.md` and do not edit them directly.
- If a target user-level file has content, migrate it into `me.md` before linking.

**Linking Hints**
- User-level memory files should be symlinks to `me.md`.
- If the user does not use a tool, skip that symlink.

**Memory Workflow**
1. Treat "remember" / "store" / "save" requests as user-level memory updates.
2. Choose a section in `me.md` that fits the memory (e.g., Persona, Workflow, Preferences, Tooling, Safety).
   - If no section fits, create a new concise section header.
3. Draft a concise memory line and ask for confirmation.
4. Update `me.md` and preserve the user's wording where possible.
5. If a link is missing, propose creating the symlink after confirming the exact path.

## Decide: Skill vs Reference Document

| Create a **Skill** when... | Create a **Reference Document** when... |
|---------------------------|----------------------------------------|
| You need repeatable agent behavior | You want to capture information for future reference |
| It defines a workflow or procedure | It's a note, tip, or knowledge snippet |
| It should be invokable by the agent | It's documentation or a cheatsheet |
