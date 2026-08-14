# Claude Code Custom Output Styles

Four custom output style profiles for Claude Code. Output styles change *how* Claude communicates (formatting, structure, verbosity) while keeping its coding abilities intact.

## The profiles

| File | Style | Best for |
|---|---|---|
| [`bluf.md`](bluf.md) | **Bottom Line Up Front** | Getting the answer/command/fix in the very first sentence, context after |
| [`st100-adhd.md`](st100-adhd.md) | **Simplified Technical English + ADHD focus** | High-signal, low-cognitive-load output: short blocks, active voice, one next step |
| [`tech-pm.md`](tech-pm.md) | **Technical Product Manager** | Architectural summaries, trade-off tables, scope/debt/timeline impact |
| [`smart-brevity.md`](smart-brevity.md) | **Smart Brevity** | Axios-style: punchy lead, "Why it matters", opt-in "Go deeper" details |

## Installation

Copy the markdown files in this folder into your global Claude Code config directory:

```bash
mkdir -p ~/.claude/output-styles
cp *.md ~/.claude/output-styles/
```

(Don't copy this README — only the four profile files.)

Alternatively, place them in a project's `.claude/output-styles/` directory to make them available only in that project.

## How to switch styles in the CLI

Once the files exist in `~/.claude/output-styles/`, switch between them inside a Claude Code session either way:

1. **Direct command:** type `/output-style bluf`, `/output-style st100-adhd`, `/output-style tech-pm`, or `/output-style smart-brevity` to hot-swap the session's style.
2. **Config picker:** type `/config`, arrow down to **Output style**, press enter, and pick a profile from the selector UI.

To go back to the default, run `/output-style default`.

## Anatomy of a style file

Each profile is a markdown file with YAML frontmatter followed by the instructions:

```markdown
---
name: my-style
description: One-line summary shown in the picker.
keep_coding_instructions: true
---

# Role and Objective
...instructions that replace the default communication style...
```

- `name` — the identifier used with `/output-style <name>`.
- `description` — shown in the `/config` picker.
- `keep_coding_instructions: true` — keeps Claude Code's built-in software-engineering behavior and only swaps the communication style.
