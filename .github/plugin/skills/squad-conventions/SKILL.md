---
name: squad-conventions
description: Coding conventions and operational rules for working in Squad-managed projects.
---

# Squad Conventions

Follow these conventions when working in a project managed by Squad.

## Agent Spawning

- **Every agent interaction MUST use the `task` tool.** Never simulate, role-play, or inline an agent's work.
- Maximize parallel work: spawn independent agents simultaneously.
- Pass `TEAM_ROOT` and the current user's name into every spawn prompt.

## File System

- Always use `path.join()` for paths. Never hardcode `/` or `\` separators.
- Use `git --no-pager` for all git commands (reliable with Windows paths).
- Write commit messages to temp files and use `git commit -F <file>` to avoid shell escaping issues.

## State Management

- `.squad/decisions.md` is append-only. Never overwrite - only append.
- Agent `history.md` files are append-only. Never overwrite.
- `.gitattributes` uses `merge=union` for these files to prevent merge conflicts.
- Runtime files (`.squad/log/`, `.squad/sessions/`, `.squad/orchestration-log/`) are not committed.

## Privacy

- Never read or store `git config user.email` (PII violation).
- Use `git config user.name` only.

## Squad-Owned vs User-Owned Files

| File | Owner | Overwrite on Upgrade |
|------|-------|---------------------|
| `.github/agents/squad.agent.md` | Squad | Yes |
| `.github/copilot-instructions.md` | Squad | Yes |
| `.squad/team.md` | User | No |
| `.squad/routing.md` | User | No |
| `.squad/agents/*/charter.md` | User | No |
| `.squad/agents/*/history.md` | User | No |
| `.squad/decisions.md` | User | No |
| `.squad/config.json` | System | Updated on upgrade |
| `.squad/casting/registry.json` | System | Migrated on upgrade |

## Zero Dependencies

- Use only Node.js builtins (fs, path, child_process, etc.).
- No external npm packages in generated code.
- All file operations must work on Windows, macOS, and Linux.
