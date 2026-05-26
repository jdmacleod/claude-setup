# Ten tips for new Claude Code users

## 1. Bootstrap CLAUDE.md with `/init`

Run `/init` in any project and Claude Code will read the codebase and generate a starter `CLAUDE.md` for you — including detected build commands, conventions, and structure. Edit it down to what's actually useful.

## 2. Describe goals, not steps

Tell Claude *what* you want to achieve, not *how* to do it. Claude can figure out the steps; giving it the goal lets it use better judgment and adapt when it hits unexpected state.

Instead of: *"Open the file, find the function, change line 42 to…"*
Try: *"The login button flickers on mobile — fix it."*

## 3. Use `/clear` between unrelated tasks

Context from earlier in a session can subtly mislead Claude on a new task. `/clear` resets the conversation without ending the session. Use it when switching to something unrelated.

## 4. Run shell commands in-session with `!`

Prefix any command with `!` in the prompt to run it in your current session and pipe the output into the conversation:

```
! git log --oneline -10
```

Useful for interactive auth commands like `! gcloud auth login` that need to run in the terminal but whose output Claude should see.

## 5. Use Plan Mode before big changes

Before Claude starts editing files, ask it to plan first. You can use `/plan` or just ask: *"What's your approach before you start?"* It's much easier to redirect a plan than to undo a half-finished refactor.

## 6. Reduce permission prompts with `/fewer-permission-prompts`

Claude Code asks for approval before running commands it hasn't seen before. Once you've established trust for common operations (running tests, reading files), run `/fewer-permission-prompts` to scan your session history and generate an allowlist in `settings.json` automatically.

## 7. Memory persists across sessions

Ask Claude to "remember" something and it will write a memory file that gets loaded in future sessions. Useful for preferences, recurring context, or team conventions that don't belong in the codebase:

*"Remember that we use Playwright for all browser tests, not Cypress."*

## 8. Spawn subagents for parallel work

Claude can delegate independent tasks to subagents running in parallel — useful for research, multi-file searches, or anything with clear boundaries. Ask for it explicitly:

*"Search the codebase for all database queries that don't use parameterized inputs — use a subagent so we don't fill the main context."*

## 9. Use `/compact` to reclaim context budget

In long sessions, Claude's context window fills up. `/compact` compresses the conversation history into a summary so you can keep working without starting over. Do it proactively before you hit the limit, not after.

## 10. Your global `~/.claude/CLAUDE.md` is always in scope

Anything in `~/.claude/CLAUDE.md` is loaded for every project, every session. Use it for universal preferences — response style, things Claude should never do, tools you always have available. Keep it short; it applies everywhere, so specificity matters less.
