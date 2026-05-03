# novel-skill

A reusable local AI skill for planning, drafting, resuming, and validating long-form Chinese novels.

This repository packages a staged novel-writing workflow around:

- guided story setup
- outline and character planning
- chapter-by-chapter drafting
- resumable project state
- automatic chapter word-count validation

## What It Does

`novel-skill` is designed for long-form Chinese fiction rather than one-shot short stories.

It helps an AI agent:

1. collect story constraints through phased questioning
2. generate a project folder with character sheets, outline, and writing plan
3. draft chapters against the outline
4. resume interrupted work from saved state
5. validate chapter length and revise weak chapters

## Repository Layout

```text
novel-skill/
├── SKILL.md
├── agents/
│   └── openai.yaml
├── references/
│   ├── flows/
│   └── guides/
└── scripts/
    └── check_chapter_wordcount.py
```

## Workflow

The skill is organized into five phases:

1. Initialization
2. Story setup
3. Planning
4. Drafting
5. Validation and repair

The detailed flow and writing guides live under `references/`.

## Output Convention

The skill writes novel projects under:

```text
./chinese-novelist/
```

Typical generated files:

- `00-人物档案.md`
- `01-大纲.md`
- `02-写作计划.json`
- `第01章-*.md`

## Script

Check a single chapter:

```bash
python3 scripts/check_chapter_wordcount.py <chapter-file>
```

Check all chapters in a project:

```bash
python3 scripts/check_chapter_wordcount.py --all <project-dir>
```

## Adaptation Notes

This repository is adapted for the current local Codex-style skill environment.

Key changes from the upstream source:

- UI-specific question-tool instructions were rewritten to plain chat-driven interaction
- the default execution mode is `serial`
- unsupported team-specific agent workflow details were removed from active usage
- command examples were normalized to `python3`

## Upstream Source

This repository is derived from:

- `PenglongHuang/chinese-novelist-skill`
- Upstream URL: https://github.com/PenglongHuang/chinese-novelist-skill

If you publish or redistribute this repository, keep upstream attribution and confirm the upstream license terms before wider distribution.
