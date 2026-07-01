# Playbook — Standard project memory structure

> Read when creating or auditing a project's memory. Not auto-loaded.

## Mandatory files (from 1st session)

Every project `~/.claude/projects/<slug>/memory/` must have :

| File | Role | Creation |
|---|---|---|
| `MEMORY.md` | Task-context index | 1st session |
| `pieges-<project>.md` | Specific traps (condensed table, 5-sec read) | 1st session (seeded) |
| `mapping-<project>.md` | Annotated code map by domain | 1st deep session |
| `dev-rules.md` | Project-adapted rules (clone reference + adapt) | 1st session |
| `lessons-learned.md` | Accumulated learnings | 1st session (empty) |
| `neural-map.md` | Mermaid extended-brain diagram | 1st deep session |
| `session-YYYY-MM-DD.md` | Significant session notes | At archival |

## Trap hierarchy

- **General** (e.g. `pieges-general.md` somewhere central) : PowerShell quirks, build, deprecated LLM models, etc. Stay general.
- **Meta** (`pieges-meta.md`) : cross-project traps about your interactions with Claude itself.
- **Project-specific** (`pieges-<project>.md`) : tied to the app type (multi-tenant DB, money-critical trading paths, real-time sync, etc.).

Rule : general stays general; what's specific to an app type goes in that project's `pieges-`.

## Optional notes (created as needed)

- `feature-<name>.md` — per significant feature
- `project-<topic>.md` — per major topic
- `feedback-<topic>.md` — when user validates/corrects a pattern
- `audit-YYYY-MM-DD.md` — for audits

## Obsidian sync (locked pattern, optional)

```powershell
# Junction (PowerShell only, not Git Bash)
New-Item -ItemType Junction `
  -Path "<obsidian-vault-path>/Claude/<Project Name> (live)" `
  -Target "~/.claude/projects/<slug>/memory"
```

+ static copy `<Project Name> (CLAUDE.md).md` (snapshot of project CLAUDE.md).

## If project exists without structure

1. Flag : *"this project doesn't have the standard structure — should I create the missing files?"*
2. Clone from a canonical reference project + adapt (techs, sensitive points, real paths)
3. Initialize `lessons-learned.md` empty, or with the lessons accumulated in the current session

## Why non-negotiable

Without it : restart each session without context → repeat the same mistakes → grep what's already mapped → 30 min lost. With it : productive from line 1.
