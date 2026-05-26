# Setup playbook

A record of deliberate configuration choices for Claude and Claude Code, with enough detail to reproduce a consistent environment across machines.

Each entry documents the rationale, current setup steps, and what needs periodic review. For a timestamped record of when each step was applied and on which machine, see [setup-log.md](setup-log.md).

## How to use this playbook

1. Work through entries in order — some have prerequisites
2. After completing an entry on a machine, add a row to [setup-log.md](setup-log.md)
3. When an upstream tool releases updates, revisit the relevant entry, update the instructions if needed, update **Last reviewed**, and add a "reviewed" log entry
4. Each entry carries a status: `current`, `needs review`, or `deprecated`

---

## ccstatusline — Claude Code status line

**Status:** current  
**Last reviewed:** 2026-05-26  
**Source:** https://github.com/sirmalloc/ccstatusline  

**Why:** Displays model name, context window percentage, session cost, git branch, and rate limit status in real time at the bottom of the Claude Code terminal. Provides at-a-glance awareness of cost and context without interrupting the session.

### Prerequisites

- Node.js (for `npx`) or Bun (for `bunx`) installed
- Claude Code ≥ 2.1.97 (required for `refreshInterval` support)

### Steps

1. Run the interactive setup TUI:

   ```bash
   npx -y ccstatusline@latest
   ```

   Or with Bun (faster):

   ```bash
   bunx -y ccstatusline@latest
   ```

2. In the TUI, select **Pinned global install**. This installs a specific version globally and writes the `ccstatusline` command to `~/.claude/settings.json`.

3. Use the TUI to configure widget layout, colors, and refresh interval. Record your widget choices in the **Configuration notes** section below so they can be reproduced quickly on another machine.

4. Confirm the settings entry was written:

   ```bash
   grep -A4 statusLine ~/.claude/settings.json
   ```

   Expected result:

   ```json
   "statusLine": {
     "type": "command",
     "command": "ccstatusline"
   }
   ```

### Verification

Open a new Claude Code session. The status line should appear at the bottom of the terminal showing model, context, and cost information in real time.

### Configuration notes

Record your widget choices here after setup, so they can be recreated without guessing:

```
widgets:  (e.g. model | flex | context % | cost | git branch)
refresh:  (e.g. 10s)
theme:    (e.g. default)
```

### Maintenance

- Check for new releases: https://github.com/sirmalloc/ccstatusline/releases
- Widget and display preferences persist at `~/.config/ccstatusline/settings.json` — if wiping a machine, export this file first
- If Claude Code updates break the status line, check the ccstatusline issues page before debugging manually
- Re-run the TUI at any time to adjust layout, colors, or refresh interval without touching `settings.json` directly
