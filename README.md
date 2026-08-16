# skills

Personal agent skills collection, usable across coding agents (Claude Code, Codex, Cursor, ZCode, and others) and installable with [skills.sh](https://skills.sh).

Each skill is a folder with a `SKILL.md` (YAML frontmatter with `name` and `description`). The folder name must match the frontmatter `name` (lowercase letters, numbers, hyphens). Supporting files live beside it in `references/`, `scripts/`, or `assets/`.

## Skills

| Skill | Purpose |
|-------|---------|
| [resolve-bug-deeply](./resolve-bug-deeply/) | Evidence-driven bug resolution: competing root-cause hypotheses, impact mapping, minimal architecture-correct fix, regression protection, adversarial review, and explicit closure gates. |
| [summary-decision-brief](./summary-decision-brief/) | Turn long or technical answers into a short decision-ready brief (Karar / Neden / Seçenekler / Trade-off / 80/20 önerisi / Sonraki adım, Turkish by default). |
| [grilling](./grilling/) | Relentlessly interview the user about a plan, decision, or idea — one question at a time, with recommended answers — until shared understanding. |
| [change-evidence-audit](./change-evidence-audit/) | Read-only, evidence-based audit of a change set, Git range, or PR before commit/push/merge: behavior-to-test mapping, boundary inspection, and a BLOCKER / REVIEW_REQUIRED / RISK_ACCEPTABLE / UNKNOWN verdict. |

## Install from this repo

```bash
npx skills add Tugser/skills            # pick skills interactively (project scope)
npx skills add Tugser/skills --all      # everything
npx skills add Tugser/skills -g         # global (all your projects)
npx skills add Tugser/skills --list     # see what's here without installing
```

## Pull skills from elsewhere

```bash
npx skills find <query>                 # search the ecosystem
npx skills add owner/repo               # install into your current project
npx skills add owner/repo --skill name  # just one skill from a repo
npx skills update                       # refresh installed skills
```

To vendor a third-party skill into this repo, install it into a scratch project with the commands above, then move the skill folder to the repo root. Keep the folder name identical to the frontmatter `name`.

## Authoring a new skill

```bash
npx skills init          # scaffold, or just create <skill-name>/SKILL.md
```

Frontmatter template:

```yaml
---
name: my-skill-name
description: What it does and when to use it, written as a trigger condition.
---
```
