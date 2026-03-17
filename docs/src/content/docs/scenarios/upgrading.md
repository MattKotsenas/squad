# Upgrading Squad

Update Squad-owned files to the latest version without touching your team state.

---

## 1. Run the Upgrade

From your repo root:

```bash
npx github:bradygaster/squad upgrade
```

Squad detects the installed version (from `.squad/config.json`'s `installedVersion` field), updates Squad-owned files, and runs any needed migrations:

```
✅ upgraded .ai-team-templates/
✅ updated .squad/config.json (installedVersion: 0.2.0)

.squad/ team state untouched — your team state is safe

Squad is upgraded. (v0.2.0)
```

> **Note:** The Squad agent prompt is delivered via the Copilot plugin (`copilot plugin install bradygaster/squad`), not as a repo-local file. To update the agent prompt, update the plugin: `copilot plugin update bradygaster/squad`.

That's it.

---

## What Gets Upgraded

| Component | Updated? | Notes |
|-----------|----------|-------|
| Squad agent prompt (Copilot plugin) | ✅ Via plugin update | Run `copilot plugin update bradygaster/squad` |
| `.ai-team-templates/` | ✅ Yes | Overwritten with latest templates |
| `.github/workflows/squad-*.yml` | ✅ Yes | Overwritten with latest squad workflows |
| `.github/copilot-instructions.md` | ⚡ Conditional | Updated only if @copilot is enabled on the team |
| `.squad/config.json` | ✅ `installedVersion` | Version stamp updated |
| `.squad/` (team state) | ❌ Never | Your team's knowledge, decisions, casting state, skills |

Squad-owned template files (`.ai-team-templates/`) are replaced entirely. Don't put custom changes in them — they'll be lost on upgrade. The agent prompt itself is delivered via the Copilot plugin and is updated separately.

Your team state in `.squad/` is never touched. Agent charters, histories, decisions, casting state, skills, and session logs are all safe.

---

## Migrations

Some upgrades require additive changes to your team state directory — like creating a new subdirectory that didn't exist in older versions.

Migrations are:
- **Additive** — they only create new files or directories, never modify existing ones
- **Idempotent** — safe to re-run; if the change already exists, it's skipped

Example: upgrading to v0.2.0 creates `.ai-team/skills/` if it doesn't already exist.

---

## Migrating .ai-team/ → .squad/ (v0.5.0+)

In Squad v0.5.0, the team state directory was renamed from `.ai-team/` to `.squad/`. Existing repos continue to work — Squad detects both. If you're still on `.ai-team/`, you'll see a deprecation warning.

**To migrate your repo:**

```bash
# Step 1: Upgrade to get the latest migration tooling
npx github:bradygaster/squad upgrade

# Step 2: Rename the directory
npx github:bradygaster/squad upgrade --migrate-directory
```

Then commit:

```bash
git add -A
git commit -m "chore: migrate .ai-team/ → .squad/"
```

**What the migration does:**
- Renames `.ai-team/` → `.squad/`
- Updates `.gitignore` and `.gitattributes` references
- Scrubs email addresses from migrated files (PII cleanup)

**Timeline:** `.ai-team/` support continues through v0.6.0. Migration becomes required in v1.0.0.

**Full details:** See the [Migration Guide](../get-started/migration.md).

---

## Version Stamping

The installed version is tracked in `.squad/config.json` (the `installedVersion` field) and can be checked from the command line:

### CLI Check

You can also check your installed version from the command line:

```bash
npx github:bradygaster/squad --version
```

The output will show your installed version (e.g., `X.Y.Z`).

---

## Already Up to Date

If you're already on the latest version:

```bash
npx github:bradygaster/squad upgrade
```

```
✅ Already up to date (v0.2.0)
```

Squad still runs any missing migrations in case a prior upgrade was interrupted.

---

## 2. Commit the Upgrade

```bash
git add .ai-team-templates/ .squad/config.json
git commit -m "Upgrade Squad to v0.2.0"
```

No changes to `.squad/` team state — the diff is limited to Squad-owned files and the version stamp.

---

## Tips

- **Upgrade is safe.** It only overwrites files that Squad owns. Your team state is never modified.
- **Agent prompt updates come via the plugin.** Run `copilot plugin update bradygaster/squad` to get the latest agent prompt. If you need custom behavior, use directives in `decisions.md` instead.
- **Re-running upgrade is harmless.** If you're not sure whether an upgrade completed, run it again. It's idempotent.
