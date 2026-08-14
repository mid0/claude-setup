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

> **Note:** the standalone `/output-style` command is **deprecated** — it was deprecated in Claude Code v2.1.73 and removed in v2.1.91. If you type `/output-style bluf` on a current version, nothing happens. Use one of the methods below instead.

Once the files exist in `~/.claude/output-styles/`, switch between them either way:

1. **Config picker:** type `/config`, arrow down to **Output style**, press enter, and pick a profile from the selector UI. Your choice is saved to `.claude/settings.local.json` in the project.
2. **Settings file:** set the `outputStyle` key directly in any settings file (`~/.claude/settings.json` for global, or a project's `.claude/settings.json` / `.claude/settings.local.json`):

   ```json
   {
     "outputStyle": "bluf"
   }
   ```

To go back to the default, pick **Default** in the `/config` menu or remove the `outputStyle` key.

**Heads-up:** the output style is baked into the system prompt at session start. After changing it, run `/clear` or start a new session for the change to take effect.

## Anatomy of a style file

Each profile is a markdown file with YAML frontmatter followed by the instructions:

```markdown
---
name: my-style
description: One-line summary shown in the picker.
keep-coding-instructions: true
---

# Role and Objective
...instructions that replace the default communication style...
```

- `name` — the identifier used in the `outputStyle` setting.
- `description` — shown in the `/config` picker.
- `keep-coding-instructions: true` — keeps Claude Code's built-in software-engineering behavior and only swaps the communication style. (Current docs spell this key with hyphens, not underscores.)
