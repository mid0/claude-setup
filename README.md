# claude-setup

**Claude Code too verbose?** Its output is more configurable than it looks.

An output style adds instructions to Claude Code's system prompt that shape *how it communicates* — structure, length, and tone. These profiles use `keep-coding-instructions: true`, so Claude Code retains its built-in software-engineering instructions. Drop a markdown file in `~/.claude/output-styles/`, pick it in `/config`, and you can reduce the preamble-then-three-paragraphs-then-bottom-line pattern.

This repo holds the profiles I use, plus the `CLAUDE.md` rules that handle the parts output styles cannot reach.

## What's here

- **[`output-styles/`](output-styles/)** — five profiles that change how Claude talks: Bottom Line Up Front, Focused Technical, Plain English, Technical PM, and a Smart Brevity-inspired profile. Includes illustrative side-by-side examples of the same answer in each style, and install instructions.
- **[`claude-md/`](claude-md/)** — rules for how Claude *writes code*, kept to one line each. Currently: the [comment rule](claude-md/), for when generated code is drowning in comments.

## Quick start

```bash
git clone https://github.com/mid0/claude-setup.git
mkdir -p ~/.claude/output-styles
cp claude-setup/output-styles/{bluf,focused-technical,plain,tech-pm,smart-brevity}.md ~/.claude/output-styles/
```

Then run `/config` in Claude Code, choose **Output style**, and pick a profile. Run `/clear` afterward — the style is baked into the system prompt at session start.

> **Note:** the standalone `/output-style` command was deprecated in v2.1.73. Use `/config`, or set `"outputStyle": "bluf"` in `~/.claude/settings.json`.

## Attribution

The `smart-brevity` profile is inspired by *Smart Brevity: The Power of Saying More with Less* by Jim VandeHei, Mike Allen, and Roy Schwartz. It is an independent adaptation for Claude Code and is not affiliated with or endorsed by Axios or the book's authors.

## More coming

I plan to add more of my global `~/.claude` setup here over time — agents, skills, hooks, permissions — so others can pick and choose what's useful.

## License

[MIT](LICENSE) — copy, modify, and use any of this freely.
