---
name: novel-skill
description: Create full-length Chinese novels through a staged workflow: preference loading, guided story setup, outline and character planning, chapter-by-chapter drafting, and automatic word-count validation. Use when the user asks to write a Chinese novel, plan and draft a long serial story, continue an unfinished novel project, or generate chaptered fiction with outlines, hooks, and character files.
---

# Novel Skill

Write complete Chinese novels in a repeatable local-project workflow.

This skill is for long-form fiction work, not short one-off stories. It creates and maintains project files under the current workspace so the novel can be resumed, validated, and extended later.

## Core Rules

Always enforce these:

1. Show, do not tell.
2. Drive each chapter with conflict or reversal.
3. End each chapter with a hook.

## Default Output Layout

Create or resume projects under:

```text
./chinese-novelist/
```

Each novel project should contain:

- `00-人物档案.md`
- `01-大纲.md`
- `02-写作计划.json`
- `第01章-...md`
- `第02章-...md`

Store cross-project preferences in:

- `./chinese-novelist/user-preferences.json`

## Workflow

Read only the needed reference file for the phase you are in.

### Phase 0: initialization

Start with [phase0-initialization.md](references/flows/phase0-initialization.md).

Do these first:

- load `./chinese-novelist/user-preferences.json` if present
- scan `./chinese-novelist/` for resumable projects
- if the user already provided enough story constraints, offer to skip part of the questionnaire

### Phase 1: story setup

Run the setup flow in three layers:

- core setup: [phase1-layer1-core.md](references/flows/phase1-layer1-core.md)
- customization: [phase1-layer2-customize.md](references/flows/phase1-layer2-customize.md)
- title selection: [phase1-layer3-title.md](references/flows/phase1-layer3-title.md)

When these references mention structured question tools, adapt them to normal chat turns in the current environment. Keep the same intent and ordering, but ask concise plain-text questions instead of relying on unavailable UI-specific tools.

### Phase 2: planning

Use [phase2-planning.md](references/flows/phase2-planning.md).

Generate:

- detailed character profiles
- chapter outline
- `02-写作计划.json`

Before writing chapters, show a concise planning summary and get user confirmation once.

### Phase 3: drafting

Use [phase3-writing.md](references/flows/phase3-writing.md).

Default to `serial` mode in this environment.

If the user explicitly asks for parallel drafting and agent delegation is available, `subagent-parallel` may be used. Do not use the `agent-teams` mode from the upstream repository in this environment.

Once drafting starts, finish the full requested run without asking for per-chapter confirmation unless the user interrupts or changes requirements.

### Phase 4: validation and repair

Use [phase4-validation.md](references/flows/phase4-validation.md).

Run automatic checks after drafting. If a chapter fails word count or obvious continuity checks, repair it before reporting completion.

## Writing Constraints

- Default chapter length: `3000-5000` Chinese characters unless the user asks otherwise.
- Always read the matching chapter plan in `01-大纲.md` before drafting a chapter.
- Keep characterization aligned with `00-人物档案.md`.
- Use the hook and dialogue references when tension is weak or the prose feels generic.
- Remove obvious AI-style phrasing during revision.

## Scripts

Use the bundled word-count checker:

```bash
python3 scripts/check_chapter_wordcount.py <chapter-file>
python3 scripts/check_chapter_wordcount.py --all <project-dir>
```

## References

Use these as needed:

- flow control and persistence rules: [shared-infrastructure.md](references/flows/shared-infrastructure.md)
- outlining and plot structure: `references/guides/outline-template.md`, `references/guides/plot-structures.md`
- character design: `references/guides/character-template.md`, `references/guides/character-building.md`
- chapter craft: `references/guides/chapter-guide.md`, `references/guides/chapter-template.md`
- hooks and suspense: `references/guides/hook-techniques.md`
- dialogue polish: `references/guides/dialogue-writing.md`
- expansion when a chapter is too short: `references/guides/content-expansion.md`
- title generation: `references/guides/title-guide.md`

## Environment Adaptation

This local skill is adapted from an upstream Claude Code skill. Apply these compatibility rules:

- replace mentions of `AskUserQuestion` with concise normal chat prompts
- ignore installation instructions meant for `~/.claude/skills` or `npx skills add`
- treat `agent-teams` as unsupported here
- when a reference suggests sub-agent work, only use it if the user explicitly wants speed and delegation is actually available
