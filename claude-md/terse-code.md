# Terse code

Rules that keep generated code free of comment bloat and defensive padding.

**Output styles cannot do this.** A style file changes how Claude talks to you. It leaves coding behavior alone by design — that is what `keep-coding-instructions: true` means. Comment ratio, dead abstractions, and needless try/except live in `CLAUDE.md` instead.

## Install

Append the block below to `~/.claude/CLAUDE.md` for every project, or to a single project's `./CLAUDE.md`.

---

```markdown
## Code style

- Comment only what the code cannot say. A comment explains *why* a choice was
  made, or flags a non-obvious constraint. Never restate the line below it.
- Delete these on sight: `// Loop through the items`, `// Set the value`,
  `# Return the result`, and any comment that repeats the function name.
- No section-banner comments (`// ==== HELPERS ====`) and no decorative rules.
- Do not add docstrings to short, self-evident private functions. Public API
  surfaces get docstrings; internal three-line helpers do not.
- Match the comment density of the surrounding file. If a file has no comments,
  do not be the first to add them.
- No `try`/`catch` unless you handle the error. Do not catch, log, and rethrow.
- No configuration knob, interface, or abstraction layer for a case that does
  not exist yet.
- Do not leave `TODO` comments for work you were asked to finish.
- When you edit an existing function, do not add comments to the lines you
  did not change.
```

---

## Why the default drifts

Claude Code copies the style it sees. Once a few heavily commented functions land in a file, later edits imitate them, and subagents imitate the subagents. One developer on Hacker News reported reaching roughly a 3:1 comment-to-code ratio after about 30 commits.

The rule that stops the spiral is "match the surrounding file". It makes the existing code the reference instead of the previous generation's output.

## Checking it worked

Count comment lines against code lines in a file Claude just touched:

```bash
# rough ratio for a JS/TS file
grep -c '^\s*//' file.ts && grep -cve '^\s*//' -e '^\s*$' file.ts
```

A ratio above about 1:5 in ordinary application code usually means the rules are not loaded. Confirm with `/memory` that Claude is reading the `CLAUDE.md` you edited.
