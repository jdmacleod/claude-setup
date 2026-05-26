# CLAUDE.md

This file serves two purposes:
1. **Instructions for Claude** — tells Claude how to work in this repo
2. **A guide for humans** — explains what CLAUDE.md files are and how to write one

---

## For Claude: how to work in this repo

**Writing style**
- Clear, concise, and actionable — every sentence should earn its place
- No emojis unless the user explicitly asks
- Use plain markdown; avoid HTML
- Second-person voice when addressing the reader ("you"), not "users" or "one"

**Conventions**
- Docs live in `docs/`; each topic gets its own file
- Headings: sentence case, not title case
- Code examples should be minimal and runnable
- No trailing summaries after completing a task — changes speak for themselves

**Before editing docs**
- Read the existing file first to match tone and structure
- Prefer editing existing files over creating new ones
- Do not create a doc unless explicitly asked

---

## What is CLAUDE.md?

`CLAUDE.md` is a markdown file that Claude Code reads automatically at the start of every session in a directory. It lets you give Claude persistent context about your project without repeating yourself every conversation.

Claude also reads `CLAUDE.md` files in parent directories and in `~/.claude/CLAUDE.md` (your global default), merging them in order from outermost to innermost.

### What to put in CLAUDE.md

**Project-specific commands** — the commands Claude should use to build, test, and lint your project:

```markdown
## Commands
- Build: `npm run build`
- Test: `npm test`
- Lint: `npm run lint`
```

**Conventions** — coding style, naming rules, patterns to follow or avoid:

```markdown
## Conventions
- Use `snake_case` for Python identifiers
- Never use `any` in TypeScript
- Prefer functional components over class components
```

**Constraints** — things Claude should never do without asking:

```markdown
## Constraints
- Do not push to `main` directly
- Do not modify `db/migrations/` without explicit approval
```

**Context** — background that helps Claude make better decisions:

```markdown
## Context
This is a monorepo. The `packages/api` and `packages/web` directories are
deployed independently. Changes to shared code in `packages/core` affect both.
```

### What not to put in CLAUDE.md

- Information already obvious from reading the code
- Explanations of *what* the code does (Claude can read it)
- Long narrative prose — Claude skims this file, so keep it scannable
- Secrets or credentials

### Tips for writing a good CLAUDE.md

- **Start with `/init`** — run `/init` in Claude Code to auto-generate a starter CLAUDE.md based on your codebase
- **Keep it short** — a focused 20-line CLAUDE.md is more effective than a 200-line one Claude has to parse
- **Update it as the project evolves** — stale instructions are worse than none
- **Test it** — start a new session and see if Claude behaves differently; adjust if not
