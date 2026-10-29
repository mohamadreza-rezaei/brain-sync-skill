# brain-sync

Agent skill (`SKILL.md`) for Cursor, Claude Code, Codex, and other coding agents.

Distills new, lasting facts from any chat or tool (via a standard YAML delta) into a second-brain repo: capture → propose diffs → apply only after approval → commit only on request.

## Install

Copy the skill folder into your agent's skills directory:

```bash
# project-local (Claude Code)
mkdir -p .claude/skills && cp -r brain-sync .claude/skills/
# global (Claude Code)
cp -r brain-sync ~/.claude/skills/
```

For other agents, put the folder wherever that agent loads skills from, or reference `brain-sync/SKILL.md` from your `AGENTS.md`.

## License

MIT
