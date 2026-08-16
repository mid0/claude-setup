# claude-setup

**Claude Code too verbose?** Its writing style is a setting, not a personality. Swap it.

An output style replaces the part of Claude Code's system prompt that governs *how it communicates* — structure, length, tone — while leaving its coding ability untouched. Drop a markdown file in `~/.claude/output-styles/`, pick it in `/config`, and the preamble-then-three-paragraphs-then-bottom-line template goes away.

This repo holds the profiles I use, plus the `CLAUDE.md` rules that handle the parts output styles cannot reach.

## What's here

- **[`output-styles/`](output-styles/)** — five profiles that change how Claude talks: Bottom Line Up Front, Simplified Technical English + ADHD focus, Plain English, Technical PM, and Smart Brevity. Includes side-by-side examples of the same answer in each style, and install instructions.
- **[`claude-md/`](claude-md/)** — rules for how Claude *writes code*, kept to one line each. Currently: the [comment rule](claude-md/), for when generated code is drowning in comments.

## Quick start

```bash
git clone https://github.com/mid0/claude-setup.git
mkdir -p ~/.claude/output-styles
cp claude-setup/output-styles/*.md ~/.claude/output-styles/
rm ~/.claude/output-styles/README.md
```

Then run `/config` in Claude Code, choose **Output style**, and pick a profile. Run `/clear` afterward — the style is baked into the system prompt at session start.

> **Note:** the standalone `/output-style` command was deprecated in v2.1.73 and removed in v2.1.91. If you type `/output-style bluf` today, nothing happens. Use `/config`, or set `"outputStyle": "bluf"` in `~/.claude/settings.json`.

## More coming

I plan to add more of my global `~/.claude` setup here over time — agents, skills, hooks, permissions — so others can pick and choose what's useful.

## License

[MIT](LICENSE) — copy, modify, and use any of this freely.
