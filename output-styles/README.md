# Claude Code Custom Output Styles

Five custom output style profiles for Claude Code. They add communication instructions to the system prompt. Each profile sets `keep-coding-instructions: true`, so Claude Code retains its built-in software-engineering instructions.

## The profiles

| File | Style | Best for |
|---|---|---|
| [`bluf.md`](bluf.md) | **Bottom Line Up Front** | Getting the answer/command/fix in the very first sentence, context after |
| [`focused-technical.md`](focused-technical.md) | **Focused Technical** | High-signal, low-cognitive-load output: short blocks, active voice, one next step |
| [`plain.md`](plain.md) | **Plain English** | Short common words, no jargon or metaphor. Written for non-native readers and for tired ones |
| [`tech-pm.md`](tech-pm.md) | **Technical Product Manager** | Architectural summaries, trade-off tables, scope/debt/timeline impact |
| [`smart-brevity.md`](smart-brevity.md) | **Smart Brevity-inspired** | Book-inspired structure: concise lead, "Why it matters", opt-in "Go deeper" details |

## What the difference looks like

Same prompt, five hand-written examples. These illustrate each profile's intended structure; they are not captured model outputs or benchmark results. The prompt:

> The `/users` endpoint got slow after yesterday's deploy. What should I check?

**Default**

> Great question — endpoint slowdowns after a deploy are a common issue, and there are several angles worth considering here. Let me walk through the most likely causes.
>
> First, it's worth understanding what changed. A deploy can affect performance through code changes, dependency updates, configuration drift, or infrastructure changes. Each of these has a different diagnostic path.
>
> One of the most frequent culprits in endpoints like `/users` is the N+1 query problem, where an ORM issues one query per record rather than a single joined query. [...three more paragraphs...]
>
> **The bottom line:** start by diffing the deploy, then check your query logs.

**`bluf`**

> Diff the deploy and check for a new N+1 query: `git diff HEAD~1 -- '*users*'`, then look at slow query logs for repeated identical `SELECT`s.
>
> ---
>
> An N+1 is the most common post-deploy cause on a list endpoint. If the diff is clean, check whether a migration dropped an index, then whether the container's memory limit changed.

**`focused-technical`**

> **Cause:** most likely an N+1 query added in yesterday's deploy.
>
> Do these in order:
>
> 1. Run `git diff HEAD~1 -- '*users*'`.
> 2. Open the slow query log. Look for the same `SELECT` repeated many times.
> 3. Check whether a migration dropped an index.
>
> ---
>
> **Next step:** run the `git diff` command above.

**`plain`**

> Check for an N+1 query first. That is when the code runs one database query per user instead of one query for all of them.
>
> Run `git diff HEAD~1 -- '*users*'` to see what changed. Then open the slow query log. If you see the same `SELECT` many times in a row, that is the problem.
>
> If the diff looks fine, check if a migration removed an index.

**`smart-brevity`**

> **Check the deploy diff for an N+1 query.**
>
> **Why it matters:** a single missing `.include()` turns one query into hundreds, and it only shows up under real row counts.
>
> **Go deeper:** `git diff HEAD~1 -- '*users*'`, then grep the slow query log for repeated `SELECT`s. Index drops from migrations are the second suspect.

`tech-pm` is not shown here — it reframes answers around scope, trade-offs, and debt, so it reads best on architecture questions rather than on debugging.

## Beyond output styles

Output styles shape the main conversation. For project conventions such as comment style and density, Anthropic recommends `CLAUDE.md`.

If your complaint is verbose *code* rather than verbose chat — comment bloat especially — see [`../claude-md/`](../claude-md/). It is one line.

## Installation

Copy the markdown files in this folder into your global Claude Code config directory:

```bash
mkdir -p ~/.claude/output-styles
cp {bluf,focused-technical,plain,tech-pm,smart-brevity}.md ~/.claude/output-styles/
```

Alternatively, place them in a project's `.claude/output-styles/` directory to make them available only in that project.

## How to switch styles in the CLI

> **Note:** the standalone `/output-style` command was deprecated in Claude Code v2.1.73. Use one of the methods below instead.

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
- `keep-coding-instructions: true` — retains Claude Code's built-in software-engineering instructions while adding the profile's communication rules. (Current docs spell this key with hyphens, not underscores.)

## Attribution

The `smart-brevity` profile is inspired by *Smart Brevity: The Power of Saying More with Less* by Jim VandeHei, Mike Allen, and Roy Schwartz. It is an independent adaptation for Claude Code and is not affiliated with or endorsed by Axios or the book's authors.

The `focused-technical` profile borrows general controlled-language ideas such as active voice, short sentences, and one topic per paragraph. It is not an implementation of, or certification against, the ASD-STE100 standard.
