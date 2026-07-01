# Getting started — 10 minutes flat

> For someone new to Claude Code. If you're already comfortable, jump to the `README.md`.

---

## What is this exactly?

A **configuration template** for Claude Code that:
- **Saves your tokens** (~50-80% less per message)
- **Keeps memory** between sessions (Claude remembers you, your projects, your mistakes/wins)
- **Personalizes itself**: on first launch, Claude asks you 10 questions and adapts the cortex to you

Instead of repeating who you are every session, Claude gets a structured "brain": an always-active cortex + memories read only when useful.

---

## Prerequisites (5 min)

- **Claude Code installed**: https://docs.claude.com/en/docs/claude-code/quickstart
- An Anthropic account (Pro or API)
- A terminal (Terminal on macOS, PowerShell on Windows, any shell on Linux)

Test that Claude Code works:
```bash
claude --version
```

If you see a version number, you're good. Otherwise, back to the Claude Code docs.

---

## Install — path #1 (fastest, recommended)

**One single step**: open Claude Code and copy-paste the prompt from **[INSTALL-PROMPT.md](INSTALL-PROMPT.md)**.

Claude detects your OS, clones the repo, copies the files to the right places, verifies, then **launches the bootstrap interview directly**. All in the same session, ~30 seconds to the first question.

If you take this path: skip "Install — path #2" and "First Hi Claude" below (the bootstrap starts on its own). Jump to **"How to check it really works"**.

---

## Install — path #2 (manual, if you want to understand)

### 1. Copy the template into your `~/.claude/` folder

**macOS / Linux**:
```bash
cd <path-to-this-folder>/claude-brain

cp CLAUDE.md ~/.claude/CLAUDE.md

mkdir -p ~/.claude/playbooks
cp playbooks/*.md ~/.claude/playbooks/
```

**Windows (PowerShell)**:
```powershell
cd <path-to-this-folder>\claude-brain

Copy-Item CLAUDE.md "$env:USERPROFILE\.claude\CLAUDE.md" -Force

New-Item -ItemType Directory -Force "$env:USERPROFILE\.claude\playbooks" | Out-Null
Copy-Item playbooks\*.md "$env:USERPROFILE\.claude\playbooks\" -Force
```

### 2. Verify the files are there

```bash
# macOS / Linux
ls ~/.claude/CLAUDE.md ~/.claude/playbooks/

# Windows PowerShell
ls "$env:USERPROFILE\.claude\CLAUDE.md"; ls "$env:USERPROFILE\.claude\playbooks\"
```

You should see `CLAUDE.md` and 7 playbooks.

---

## First "Hi Claude!" (2 min)

Launch Claude Code in any folder:
```bash
claude
```

Then type exactly:
```
Hi Claude!
```

**What should happen**:
1. Claude detects that your cortex still contains `<TO FILL>`
2. It launches the **bootstrap interview**: 10 short questions (name, job, OS, stack, preferred style, active projects, etc.)
3. Answer one at a time, naturally (no perfect answers needed — you can edit later)
4. At the end, Claude **edits `~/.claude/CLAUDE.md` itself** with your answers
5. It confirms: *"Your cortex is configured"*

**Congrats — your Claude is now YOUR Claude.**

---

## How to check it really works

### Test 1 — memory persists
Quit Claude Code (`Ctrl+C` or `exit`). Relaunch it. Type:
```
Hi Claude! Do you remember me?
```
It should answer using your name and context (not ask who you are again).

### Test 2 — the trigger map works
Type:
```
I want to start a new project.
```
Claude should **read `playbooks/nouveau-projet.md`** before answering (instead of improvising).

### Test 3 — token savings
Check Claude Code's usage (`/cost` or `/status` depending on your version) before and after — the difference should be visible on similar questions.

---

## Common issues

| Symptom | Likely cause | Fix |
|---|---|---|
| Claude doesn't run the interview at the first "Hi Claude!" | `~/.claude/CLAUDE.md` wasn't loaded | Check it exists: `cat ~/.claude/CLAUDE.md` (must contain `<TO FILL>`) |
| Claude doesn't open playbooks on trigger | Wrong path in the cortex | The cortex points to `~/.claude/playbooks/...` — check the files are there |
| Windows errors with `&&` | PowerShell 5.1 doesn't support it | At bootstrap, answer that your OS is "Windows / PowerShell 5.1" — Claude will adapt |
| The cortex feels too long after bootstrap | Normal at first | After a few sessions, ask: *"run a self-introspection"* — Claude will measure and propose cuts |

---

## What next?

### Create your first project's memory
Once inside a project, tell Claude:
```
Let's set up the memory structure for this project.
```
It will read `playbooks/structure-memoire-projet.md` and create the standard files (`MEMORY.md`, `dev-rules.md`, `pieges-<project>.md`, `lessons-learned.md`, `wins.md`).

### The 2 files that change everything: `lessons-learned.md` + `wins.md`
- Every time you **solve a non-trivial bug** → Claude must add an entry to `lessons-learned.md`
- Every time you explicitly validate an approach ("great idea", "perfect") → entry in `wins.md`
- At the next "Hi Claude!", it re-reads the last 80 lines of each → it stops repeating mistakes AND builds on what works

This is what turns Claude from an "AI assistant" into a **partner who knows you**.

### Make it yours
The cortex is YOURS. If a rule annoys you, delete it. If you want a new trigger, add it. Ask Claude:
```
I'd like you to do X every time Y happens. Add the trigger.
```

### When Claude feels slow or burns too many tokens
```
Run a self-introspection.
```
It will measure your cortex size, active MCPs, project memories — and propose 1-3 concrete optimizations.

---

### Bonus: give your Claude a clock (recommended)

Claude has **no sense of time** — it can wish you "good night" at 2pm. The clean fix: a hook that stamps the current time onto every message you send. Add this to `~/.claude/settings.json` (merge with existing content, don't overwrite):

```json
{
  "hooks": {
    "UserPromptSubmit": [
      {
        "hooks": [
          {
            "type": "command",
            "command": "date \"+⏰ %A %d %B %Y — %H:%M (local time)\""
          }
        ]
      }
    ]
  }
}
```

Result: every message reaches Claude with the fresh time — it situates itself without ever guessing. (You can also ask Claude to install it: *"add a UserPromptSubmit hook that injects the date and time"*.)

---

## Need help?

Everything is in the `README.md` (brain model explanation) and the playbooks (which self-trigger when relevant).

Enjoy the ride. 🚀
