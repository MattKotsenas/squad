---
name: squad-init
description: Bootstrap a new Squad team in the current project using the Squad CLI.
---

# Squad Init - Project Bootstrapping

Use this skill when no `.squad/` directory exists and the user wants to set up an AI team.

## Step 1: Ask About Config Format

Before scaffolding, ask the user which configuration style they prefer:

- **Markdown (default)** - Configuration lives in `.squad/` markdown files. Simpler, no build step.
- **SDK (TypeScript)** - Generates `squad.config.ts` with typed builders (`defineSquad()`, `defineTeam()`, `defineAgent()`). Requires `npx @bradygaster/squad-cli build` after config changes.

Use `ask_user` to let them choose. Default to markdown if they have no preference.

## Step 2: Scaffold

Run the CLI to create the project skeleton:

```
npx @bradygaster/squad-cli init
```

Or for SDK mode:

```
npx @bradygaster/squad-cli init --sdk
```

This creates `.squad/` directory structure, `.github/agents/squad.agent.md`, GitHub workflows, and initial configuration. The command is non-interactive and exits cleanly.

## Step 3: Verify and Continue

1. Read `.squad/team.md` to confirm the scaffold was created
2. If `## Members` section is empty, proceed with the casting ceremony (propose a team)
3. If a roster already exists, switch to Team Mode

## Other CLI Commands

These commands are available for ongoing management:

| Command | Purpose |
|---------|---------|
| `npx @bradygaster/squad-cli upgrade` | Update Squad-owned files to latest version |
| `npx @bradygaster/squad-cli doctor` | Check squad health and diagnostics |
| `npx @bradygaster/squad-cli build` | Compile `squad.config.ts` into `.squad/` markdown (SDK mode) |
| `npx @bradygaster/squad-cli export` | Export team state to portable JSON |
| `npx @bradygaster/squad-cli import <file>` | Restore team from export |
| `npx @bradygaster/squad-cli copilot` | Add @copilot coding agent to team |
