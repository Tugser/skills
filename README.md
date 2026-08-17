# skills

Personal agent skills collection, usable across coding agents (Claude Code, Codex, Cursor, ZCode, and others) and installable with [skills.sh](https://skills.sh). Also carries the canonical global [`AGENTS.md`](./AGENTS.md) with identity, communication, and Git safety rules for Tugser's agents.

Each skill is a folder with a `SKILL.md` (YAML frontmatter with `name` and `description`). The folder name must match the frontmatter `name` (lowercase letters, numbers, hyphens). Supporting files live beside it in `references/`, `scripts/`, or `assets/`.

## Skills

| Skill | Purpose |
|-------|---------|
| [resolve-bug-deeply](./resolve-bug-deeply/) | Evidence-driven bug resolution: competing root-cause hypotheses, impact mapping, minimal architecture-correct fix, regression protection, adversarial review, and explicit closure gates. |
| [summary-decision-brief](./summary-decision-brief/) | Turn long or technical answers into a short decision-ready brief (Karar / Neden / Seçenekler / Trade-off / 80/20 önerisi / Sonraki adım, Turkish by default). |
| [grilling](./grilling/) | Relentlessly interview the user about a plan, decision, or idea — one question at a time, with recommended answers — until shared understanding. |
| [change-evidence-audit](./change-evidence-audit/) | Read-only, evidence-based audit of a change set, Git range, or PR before commit/push/merge: behavior-to-test mapping, boundary inspection, and a BLOCKER / REVIEW_REQUIRED / RISK_ACCEPTABLE / UNKNOWN verdict. |

## Global AGENTS.md

[`AGENTS.md`](./AGENTS.md) at the repo root is the canonical global agent instruction file: identity, communication style, core working principles, skill routing, Git/GitHub safety, secrets, and completion gates. Skills own specialized workflows; `AGENTS.md` owns broad behavior and safety rules.

Install it globally with a symlink so edits stay in sync with this repo:

```bash
git clone https://github.com/Tugser/skills.git ~/Documents/GitHub/skills

ln -sf ~/Documents/GitHub/skills/AGENTS.md ~/.claude/AGENTS.md   # Claude Code
ln -sf ~/Documents/GitHub/skills/AGENTS.md ~/.codex/AGENTS.md    # Codex
ln -sf ~/Documents/GitHub/skills/AGENTS.md ~/.zcode/AGENTS.md    # ZCode
ln -sf ~/Documents/GitHub/skills/AGENTS.md ~/.agents/AGENTS.md   # OpenClaw-style agents
```

Notes:

- Adjust the clone path if you keep the repo elsewhere; the links just need to point at the cloned `AGENTS.md`.
- If a target file already exists (e.g. a tool-generated `~/.codex/AGENTS.md`), review its content before replacing it with the link.
- The same file works for any agent that reads a global `AGENTS.md`; add more links as needed.

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
