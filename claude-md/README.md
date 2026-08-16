# CLAUDE.md rules

Output styles change how Claude *talks*. They leave code generation alone by design — that is what `keep-coding-instructions: true` means. Comment bloat is a `CLAUDE.md` problem.

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

Claude Code imitates the style it reads. A few heavily commented functions land in a file, later edits copy them, and subagents copy the subagents. One developer on Hacker News reported reaching roughly a 3:1 comment-to-code ratio after about 30 commits.

*Match the file's existing density* is the clause that stops the ratchet.
