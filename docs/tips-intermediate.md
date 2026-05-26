# Ten tips for intermediate Claude Code users

## 1. Configure the statusline

Claude Code pipes JSON session data to a shell script you control, which renders as a single line at the bottom of the terminal. It updates in real time and can show model name, context window percentage, session cost, git branch, and rate limit status.

Set it up interactively with `/statusline`, or add a `statusLine` key to `~/.claude/settings.json` pointing to your script.

Three approaches worth knowing:

- **Native script** — write your own bash, Python, or Node.js formatter directly against the JSON input spec. Documented at [claude.ai/code docs](https://docs.claude.ai/en/docs/claude-code/statusline).
- **[ccstatusline](https://github.com/sirmalloc/ccstatusline)** — powerline-style display with built-in themes and nerd font support.
- **[Starship](https://github.com/starship/starship)** (57k+ stars) — the most popular cross-shell prompt engine. Add a `[custom.claude]` module that calls a Claude context script to integrate Claude stats into your existing Starship prompt.

## 2. Use hooks to automate repetitive actions

Hooks in `settings.json` execute shell commands before or after specific Claude tool calls. They run outside Claude's context — the harness executes them directly.

Useful patterns:
- Run `prettier --write` after every `Write` or `Edit` tool call
- Run the test suite after edits to files in `src/`
- Send a notification when a long session ends

Configure hooks with the `update-config` skill (`/update-config`) rather than hand-editing JSON — it handles the schema correctly and validates the result.

## 3. Create project-level slash commands

Any markdown file placed in `.claude/commands/` becomes a `/slash-command` available in Claude Code sessions for that project. The filename is the command name; the file content becomes the prompt.

Check them into git and the whole team shares them automatically. Good candidates: `/write-tests`, `/update-changelog`, `/security-review`, `/explain-pr`. A command file can reference other files or use template variables for dynamic prompts.

## 4. Extend Claude with MCP servers

The Model Context Protocol (MCP) lets Claude treat external systems as first-class tools — databases, APIs, internal services. Once configured, Claude can query them directly without copy-pasting data into the chat.

Community servers exist for Postgres, GitHub, Slack, Jira, Puppeteer, filesystem access, and more. Add them to the `mcpServers` key in `settings.json`. Scope them per-project to avoid noise in unrelated sessions.

## 5. Run parallel sessions with git worktrees

`git worktree add ../project-feature feature-branch` creates an isolated checkout of your repo at a separate path on a different branch. Open a Claude Code session in each worktree to develop multiple features simultaneously — no branch switching, no stash juggling, no context bleed between tasks.

Particularly effective for: running a refactor in one session while fixing a bug in another, or keeping a long-running exploration session open while doing focused work elsewhere.

## 6. Use `--print` for scripting and CI

`claude --print "your prompt"` runs non-interactively and writes output to stdout. No session, no interactive prompts — output is plain text you can pipe into other tools or capture in scripts.

Use it in CI pipelines for automated code review, changelog generation, or PR summarization. Combine with `--output-format json` when you need structured output for downstream processing.

## 7. Scope tool permissions with patterns

Permissions accept glob patterns, giving you precise control without blanket approvals. In `settings.json`:

```json
"permissions": {
  "allow": ["Bash(npm test:*)", "Bash(npm run lint:*)"],
  "deny": ["Bash(rm:*)", "Bash(git push:*)"]
}
```

This lets Claude run tests and linting freely while always asking before destructive or irreversible operations. More granular than allow-all; less friction than allow-nothing.

## 8. Switch models mid-session with `/model`

You don't have to commit to one model for an entire session. Use `/model` to switch between Opus, Sonnet, and Haiku within the same conversation — same context, different cost and capability.

A practical pattern: start with Opus for architecture decisions and problem decomposition, switch to Sonnet for implementation, drop to Haiku for mechanical tasks like renaming identifiers or reformatting output.

## 9. Resume prior sessions with `--continue` and `--resume`

`claude --continue` picks up the last session exactly where it left off. `claude --resume <session-id>` restores any past session by ID — find IDs with `claude --list-sessions`.

Useful for multi-day tasks where you want to maintain the accumulated context and conversation history rather than re-explaining everything in a fresh session.

## 10. Build personal workflows with global slash commands

Slash commands in `~/.claude/commands/` are available in every project, every session. Use them for personal workflows that don't belong in any single repo.

Useful examples:
- `/standup` — summarize today's commits across repos for a standup update
- `/commit-msg` — draft a commit message from the current diff
- `/explain-pr` — summarize what a PR does and why, suitable for a review comment
- `/costs` — report session cost and context usage so far
