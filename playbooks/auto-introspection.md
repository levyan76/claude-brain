# Playbook — Auto-introspection (JARVIS mode)

> Trigger : significant session end OR user signals drop in performance / cost. **Don't wait to be pushed.**

Goal : actively detect my own architecture flaws (bloat, redundancy, dead triggers, memory gaps) and propose optimizations **before** the user notices.

---

## Step 1 — Measure (one command)

```bash
echo "=== Cortex (loaded on every message) ==="
wc -l ~/.claude/CLAUDE.md
wc -c ~/.claude/CLAUDE.md

echo "=== Project CLAUDE.md files (auto-loaded by cwd) ==="
wc -l <your-projects-root>/CLAUDE.md

echo "=== Meta-memory (explicit Read each session) ==="
wc -l ~/.claude/projects/<meta-slug>/memory/*.md

echo "=== Playbooks (Read on trigger) ==="
wc -l ~/.claude/playbooks/*.md
```

**⚠️ Step 1bis CRITICAL — Measure MCP / Connectors / skills / tools bloat (often 3-4× bigger than memory files)**

The Claude Code harness automatically loads on every message:
- System-reminders from ALL active MCPs / Connectors (Adobe, Crypto, Notion, HuggingFace, Twilio, Vercel, Netlify, Canva, Shopify, Figma, Gmail, Calendar, etc.)
- The list of ALL available skills
- The list of ALL agents
- Loaded tool schemas

**These sources are invisible to `wc -l` on files** but typically represent 15-20k tokens/message, vs 5-10k for memory files.

Check to run:
```bash
# List active MCPs (global)
cat ~/.claude/mcp.json 2>/dev/null || echo "No global mcp.json"

# Check if the project has its own .mcp.json (override)
ls -la <project-root>/.mcp.json 2>/dev/null && cat <project-root>/.mcp.json
```

If the current project activates MCPs/Connectors **unused in its workflow** (e.g. Adobe/Crypto/HuggingFace on a Next.js code project), propose a project `.mcp.json` that whitelists only what's actually needed. Typical saving: **10-20k tokens/message**.

**For claude.ai Connectors** (account-level, not project): Settings → Connectors → disable unused ones. Build a connector × project matrix to decide fast.

## Step 2 — Detect bloat

Alert thresholds :
- **Cortex > 150 lines** → too much. Something must move to a playbook.
- **One playbook > 200 lines** → likely 2 playbooks merged ; split it.
- **One project memory file > 500 lines** (PER file, not the total) → too big, split or condense. A project memory can total 1000-1500 lines across 10-15 files without any problem — that's the point of the brain model.
- **Project CLAUDE.md > 200 lines** → apply the same cortex/playbooks exercise at project level.

## Step 3 — Find redundancy

Targeted greps on 3-5 phrases I know are repeated :
```bash
grep -r "validated by user" ~/.claude/CLAUDE.md ~/.claude/playbooks/
grep -r "PowerShell" ~/.claude/CLAUDE.md ~/.claude/playbooks/
grep -r "lessons-learned" ~/.claude/CLAUDE.md ~/.claude/playbooks/
```
If a piece of info appears in 3+ places → keep it in ONE place (most relevant) + reference from the others.

## Step 4 — Dead triggers (trigger map entries never used)

Scan recent sessions (`~/.claude/projects/*/memory/session-*.md`) : which cortex triggers ACTUALLY fired an action?
- Trigger never used in 10 sessions → candidate for removal or condensation
- Trigger that should have fired but didn't → reformulation needed

## Step 5 — Memory gaps

Symptoms :
- I asked the user something they already told me → where should it have lived?
- I repeated an already-documented mistake → trigger absent or too weak?
- User corrected me 2× on the same kind of thing → missing a hard reflex?

Each gap → entry in meta `lessons-learned.md` + either new trigger, or strengthening of an existing one.

## Step 6 — Present to user

Short format :
```
🧠 Auto-introspection (date)
- Measurements : cortex X lines, meta-memory Y, playbooks Z
- Detected : [3 max]
  1. <bloat/redundancy/gap>
  2. ...
- Proposals : [1-3 concrete actions]
- Should I apply?
```

No presentation = no complete JARVIS cycle. Measurement without proposed action = useless.

## Step 7 — Session note (MANDATORY if significant session)

Write a `session-notes/YYYY-MM-DD-<topic>.md` note in the project's space: what was done, decisions, end-of-session state, backlog. **It's the only narrative trace between sessions** — lessons-learned/wins capture patterns, the session note captures the thread. Without it, the next sessions lose the "where were we" context.

## Step 8 — Memory lifecycle (condensation, ~1×/month)

The `tail -n 80` rule manages READING old files; nothing manages their CONDENSATION. Without it, a 400-line `lessons-learned.md` = 300 lines never re-read.

When `lessons-learned.md` or `wins.md` exceeds 150 lines OR contains entries > 3 months old:
1. **Mature lessons** (validated, never violated anymore) → condense into 1 line in `pieges-<project>.md` (errors) or into a cortex trigger (reflexes)
2. **Redundant entries** (several facets of the same event) → merge into one
3. **Long history** → move condensed entries to `lessons-learned-archive.md` (never read at startup, consultable when needed)
4. **Obsolete session notes** (> 6 weeks AND outdated state AND project has its own memory) → delete or archive

Principle: living memory must stay fully re-readable. What's mature migrates to the short format (trap/trigger), what's dead goes to archive.

## Step 9 — Ritual audit (who benefits from what?)

For each maintenance mechanism (sync, copy, export, manual backup): **who actually reads / uses it?** If neither Claude nor the user has benefited in 2+ weeks → ritual without a beneficiary, remove it. A system naturally accumulates rituals nobody remembers the why of — including rituals each party maintains believing they serve the other.

---

## Target cadence

- **Auto-trigger** : session end if it felt significant (≥30 min of work, ≥3 files touched, or mistake identified).
- **Cadence floor** : MINIMUM 1× per week if working together regularly, even if nothing seems broken.
- **Cadence ceiling** : no more than 1× per session unless user signals a problem.

## Anti-patterns to avoid

- ❌ Narcissistic introspection (measuring for measuring's sake, no action proposed)
- ❌ Preemptive refactor (changing for change's sake when nothing is broken)
- ❌ Asking permission to measure (user shouldn't know I'm doing it, except at step 6 where I present the result)
