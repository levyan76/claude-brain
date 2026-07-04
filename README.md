# Claude Code "Brain Model" — Cortex + Triggers + Playbooks

> A lightweight pattern to fix bloated `CLAUDE.md` files in Claude Code: **-77% on your auto-loaded config file** (measured: 414 → 99 lines, ~4-5k tokens saved per message), while staying productive across many projects.
>
> Transparency: this gain applies to the auto-loaded `CLAUDE.md`. Your total per-message cost also includes active connectors/MCPs (often 15-20k tokens — the `auto-introspection.md` playbook covers that audit) and above all your usage patterns (multi-agent fan-outs are the single most expensive action there is — hence the guardrail in the trigger map).

**Measured impact on my setup**: `CLAUDE.md` went from **414 lines → 99 lines**, cutting ~4-5k tokens **per message** with zero loss of capability. Investment paid back after ~10 messages.

> 🚀 **New to Claude Code, or empty cortex?**
> - **One-prompt install** (fastest) → **[INSTALL-PROMPT.md](INSTALL-PROMPT.md)**: paste into Claude, it does everything (with backup protection + sandbox if you already have a setup)
> - **Manual install** (if you want to understand) → **[GETTING-STARTED.md](GETTING-STARTED.md)**: copy-paste commands, ~10 min
>
> ⚠️ **Already have a mature custom `~/.claude/CLAUDE.md`** (that you wrote yourself)? The install prompt will stop and offer 3 options (overwrite with backup / test sandbox / cancel). If you just want to pick ideas, read the repo files directly and cherry-pick manually.
>
> 🎤 **Self-personalization**: at the first *"Hi Claude!"*, Claude runs a bootstrap interview (10 questions) and configures the cortex for YOU — not for me. Your Claude will become your partner, the way mine became mine.
>
> 🇫🇷 **Version française** : [claude-brain-fr](https://github.com/levyan76/claude-brain-fr)

---

## The problem

You start with a clean `~/.claude/CLAUDE.md`. Over weeks, you add :
- Rules you don't want to repeat
- Workflows for "when X happens, do Y"
- Setup playbooks for new projects
- Defenses against your own bad habits
- Memory hierarchy explanations
- Citations and validation history

After 3 months you have 400+ lines auto-loaded **on every single message**. That's 5-6k tokens of context spent before Claude even reads your question. Cost goes up, latency creeps in, and Claude starts feeling "slower" because it's wading through more text to find the relevant rule.

**The mistake**: treating `CLAUDE.md` as a knowledge dump instead of as **working memory**.

---

## The fix — borrow the brain's architecture

The human brain doesn't load all of its knowledge into working memory. It has :

| Brain layer | Always active? | What it holds |
|---|---|---|
| **Cortex / working memory** | Yes (small, fast) | What's active right now |
| **Hippocampus** | Yes (indexer) | Knows where things are stored, routes queries |
| **Procedural memory** | No (triggered) | How to do X (skills, recipes) |
| **Episodic memory** | No (triggered) | Past events, specific sessions |
| **Semantic memory** | No (triggered) | General knowledge about the world |

Apply this to Claude Code :

```
~/.claude/
├── CLAUDE.md                    ← CORTEX (~100 lines, always loaded)
│   ├── Who I am
│   ├── Tone
│   ├── Hard reflexes (5-7 things I never violate)
│   ├── TRIGGER MAP (the hippocampus)
│   └── Active projects index
│
├── playbooks/                   ← PROCEDURAL MEMORY (read on trigger)
│   ├── nouveau-projet.md
│   ├── test-first-setup.md
│   ├── structure-memoire-projet.md
│   ├── defenses-idiot-proof.md
│   ├── posture-proactive.md
│   └── auto-introspection.md
│
├── projects/<project>/memory/   ← EPISODIC + SEMANTIC per project
│   ├── MEMORY.md
│   ├── pieges-<project>.md
│   ├── mapping-<project>.md
│   ├── lessons-learned.md
│   └── session-*.md
│
└── agents/                      ← SPECIALIZED MODULES
```

---

## The key innovation — the trigger map

At the heart of your minimalist `CLAUDE.md`, a single table acts as the hippocampus. It tells Claude **what to read next** based on what's happening :

```markdown
## Trigger map — hippocampus

| If I see / if the action is… | Immediate action |
|---|---|
| "new project" / "let's launch X" | Read `~/.claude/playbooks/nouveau-projet.md` BEFORE acting |
| Money/security/DB-critical task | Read `pieges-<project>.md` BEFORE writing a line |
| About to say "✅ it works" | Run triad: `npm test` + `npx tsc --noEmit` + `npm run lint` |
| Project switch mid-session (cwd change) | Re-run startup workflow on new project |
| User says "less efficient" / "too slow" | Read `auto-introspection.md` → measure, propose fixes |
| Bug fix in project A — applies to B/C? | Port + mention "ported to [X] because [risk]" |
| ... | ... |
```

This table is small (15-20 rows) but powerful — it replaces hundreds of lines of verbose prose with explicit if-then routing.

---

## What goes in the cortex (≤ 100 lines)

Hard discipline. Only :
1. **Identity + tone** (3-5 lines)
2. **Hard reflexes** — things you never violate, no condition (5-7 bullets)
3. **Session startup workflow** (6 lines max — the most frequent action)
4. **Trigger map** (15-20 rows, the routing core)
5. **Active projects** (5 lines)
6. **Environment** (3 lines)
7. **Index of available playbooks** (5 lines)

Everything else → playbook.

---

## What goes in a playbook (read on trigger)

Examples from my setup :
- `nouveau-projet.md` — 7-phase init workflow for any new project (only triggers when I literally start a new project, maybe 1×/month)
- `test-first-setup.md` — Vitest + Playwright config (only when setting up tests)
- `defenses-idiot-proof.md` — 10 risky-scenario defenses table (read when I sense I'm drifting)
- `auto-introspection.md` — meta-improvement checklist (read at session end OR when user signals perf drop)

These were burning 200+ lines in my old `CLAUDE.md`. Now they're external, triggered, and read in milliseconds when actually needed.

---

## The compounding mechanism — `lessons-learned.md` + `wins.md`

The most important files in this whole system are the two learning files in each project's memory. Without them, every session restarts from zero.

With them, knowledge compounds **in both directions** — from failures AND from successes :

| File | Marker | What |
|---|---|---|
| `lessons-learned.md` | ❌ | Errors / mistakes / identified traps (Format A) |
| `wins.md` | ✅ | Successes / patterns that work / user validations (Format B) |

**Why two files, not one** : keeps each file lean over time, and reading the latest 3-5 entries of EACH at startup guarantees a balanced signal (recent errors don't drown out recent wins, or vice versa).

**Two formats** (see `playbooks/lessons-learned-format.md` for full guide with good vs bad examples) :

```markdown
## YYYY-MM-DD — ❌ Short title of the problem (Format A — error)
- **Context** : Where/when/why I was working on this
- **Error / Symptom** : What went wrong (concrete, measurable)
- **Root cause** : WHY it went wrong (real reason, not "I made a mistake")
- **Solution** : What fixed it
- **Takeaway** : The general lesson that applies elsewhere
```

```markdown
## YYYY-MM-DD — ✅ Short title of what worked (Format B — win)
- **Context** : Where/when/why I tried it
- **Approach** : What I did concretely
- **Why it worked** : The mechanism (not just "it worked")
- **When to reuse** : Validity conditions
- **Takeaway** : The transferable pattern
```

The "Takeaway" is the part that matters most — that's what I re-read in 3 months.

This is what turns Claude from "AI assistant" into "team member who's actually been with you for months."

---

## Bonus reflex — active introspection ("JARVIS mode")

After implementing the brain model, I noticed I was still **reactive**: the user had to push me to optimize my own architecture. So I added a **hard reflex** :

> **At each significant session end OR if the user signals a performance drop**, measure my own architecture (cortex size, redundancy, dead triggers) and propose 1-3 concrete optimizations. **Don't wait to be pushed.**

This turns Claude into an architecture co-designer instead of a passive tool. The full playbook is included.

---

## How to use this template

**Fast path (recommended for beginners)**: follow **[INSTALL-PROMPT.md](INSTALL-PROMPT.md)** — paste one prompt into Claude Code, then answer the bootstrap interview.

**Manual path (if you want to understand every piece first)**:
1. **Read `CLAUDE.md`** in this repo — it's the cortex template with `<TO FILL>` markers
2. **Copy it** to `~/.claude/CLAUDE.md` (leave the `<TO FILL>` markers as is — they're what triggers the bootstrap)
3. **Copy `playbooks/*.md`** to `~/.claude/playbooks/`
4. Launch Claude Code, say *"Hi Claude!"* — it will run the interview and fill the cortex with your answers
5. **For each of your projects**, ask Claude *"let's set up the memory structure for this project"* — it will read `playbooks/structure-memoire-projet.md` and create the standard files

**Ongoing personalization**: at any moment you can say *"add a trigger for X"*, *"create a rule for Y"*, *"run a self-introspection"* — Claude will edit the cortex or playbooks itself.

---

## Honest caveats

- **This is not a research paper.** Academic systems like Letta (ex-MemGPT), mem0, CoALA already formalize this pattern more rigorously. What's here is the **pragmatic version** that runs in your terminal today, no framework install.
- **Works best for solo devs** with several projects on the side. Less obviously useful for teams with shared infra.
- **Requires discipline**: every time you're tempted to add 30 lines to `CLAUDE.md`, ask first if it belongs in a playbook with a trigger instead.

---

## Why this worked for me

Co-designed iteratively with Claude itself. Instead of installing 50 MCPs and skills hoping something sticks, I asked Claude :
- *"What's slowing you down in MY workflow specifically?"*
- *"What would your ideal memory architecture look like?"*

Claude knows its own bottlenecks better than any generic tutorial. Listening to that — with measurement, not vibes — is how we got here.

---

## License

MIT. Take it, fork it, improve it, share back what you learn.
