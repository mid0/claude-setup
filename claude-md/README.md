# CLAUDE.md rules

Output styles shape the main conversation. Project conventions such as comment style and density belong in `CLAUDE.md`; `keep-coding-instructions: true` retains Claude Code's built-in software-engineering instructions.

`CLAUDE.md` loads into every prompt in the project, so every line costs you context forever. Keep it to rules that pay rent.

## The comment rule

Add this one line to `~/.claude/CLAUDE.md`:

```markdown
- Comments explain why, never what. Don't echo the code; match the file's existing comment density.
```

That is the whole thing.

## Why this line

It is three rules from Kernighan and Plauger's *The Elements of Programming Style* (1974), compressed:

- **"Don't echo the code."** Kills `// increment i` and every comment that restates the line below it.
- **"Don't over-comment."** Handled by *match the file's existing density* — the surrounding file becomes the reference, so Claude cannot ratchet upward edit after edit.
- **Why, not what.** Leaves room for the comments that actually matter: the non-obvious constraint, the reason a slower path was chosen.

Fifty years of practice have not dated it, and it costs about 20 tokens.

## The failure it prevents

Claude Code can follow the style it sees in a file. When heavily commented functions become the local example, later edits can add even more commentary.

*Match the file's existing density* tells Claude to preserve the project's current convention instead of introducing a new one.
