# Playbook — New project from scratch

> Read only when user says "new project", "let's launch X", "from scratch". Not auto-loaded.

## Phase 1 — Scoping (3-5 questions BEFORE any file)

1. Nature (web app / desktop / API / mobile / lib / CLI / marketing site)?
2. Stack (Next.js / Vite+React / Node Express / Supabase / other)?
3. Audience (B2B SaaS / personal app / internal tool / quick MVP vs serious prod)?
4. Existing code to analyze **or** start from zero?
5. Name + path (often `<your-projects-root>/<name>/`)?

## Phase 2 — Memory structure (auto, I inform in passing)

In `~/.claude/projects/<slug>/memory/`, create in this order :

| File | Initial content |
|---|---|
| `MEMORY.md` | Index + "What to read by task" table |
| `pieges-<project>.md` | Seed generic stack traps (Supabase RLS, trading idempotence, Next.js hydration…) |
| `dev-rules.md` | Clone reference project + adapt (money, multi-tenant, etc.) |
| `lessons-learned.md` | Empty template with format Context/Error/Cause/Solution/Takeaway |
| `neural-map.md` | Mermaid with generic nodes (user, Claude, memory, code, MCP) |
| `mapping-<project>.md` | Deferred until first code exploration |

## Phase 3 — Optional Obsidian sync (PowerShell on Windows)

```powershell
New-Item -ItemType Junction `
  -Path "<obsidian-vault-path>/Claude/<Project Name> (live)" `
  -Target "~/.claude/projects/<slug>/memory"
```
+ static copy `<Project Name> (CLAUDE.md).md` in vault.

## Phase 4 — Project CLAUDE.md at project root

Create `<your-projects-root>/<name>/CLAUDE.md` with :
- Banner "🧠 project memory MANDATORY" at top
- Identity (name, type, GitHub, priority)
- **Real** stack (not invented)
- Structure map
- Dev/test/build commands
- Domain-specific sensitive points
- State as of YYYY-MM-DD

## Phase 5 — Test-first setup (before 1st feature commit)

```bash
npm install -D vitest @vitest/ui jsdom @testing-library/react @testing-library/jest-dom
npm install -D @playwright/test @axe-core/playwright
npx playwright install chromium
```

Configs : `vitest.config.ts` (alias @/, jsdom), `playwright.config.ts` (baseURL + mobile-chrome), `test-setup.ts` with `@testing-library/jest-dom`. `package.json` scripts : `test`, `test:e2e`, `test:all`.

**3 mandatory first tests** :
1. Smoke E2E (root / login / 404 don't crash)
2. 1 unit test on 1 critical business function
3. axe-core a11y test if public front

## Phase 6 — Add to global profile

"Active projects (priority order)" section of global CLAUDE.md. Propose placement, user validates.

## Phase 7 — Short final recap

```
✅ Project <name> initialized.
- Project memory : 5 seed files, Obsidian sync active
- Tests : Vitest + Playwright + smoke E2E
- Project CLAUDE.md created + added to global profile

What do we build first?
```

## When to SKIP this playbook

- "30-min POC" / "throwaway script" → minimal setup (just basic project CLAUDE.md)
- External lib / third-party tool we don't maintain

## Special case — cloning an existing repo to analyze

1. Phase 1 quick scope (continue / redo / archive?)
2. Phase 2 + 3 (empty memory + Obsidian)
3. **Skip Phase 5** (tests may already exist — audit first)
4. Light audit : structure / stack / sensitive points / tech debt → 5-bullet summary
5. Ask user : continue / redo / archive?
