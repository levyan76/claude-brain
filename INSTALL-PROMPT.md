# One-prompt install

> The fastest path. Copy-paste the block below into a fresh Claude Code session and let it guide you.

---

## ⚠️ Who this prompt is for

- ✅ **You're new to Claude Code** OR **your `~/.claude/CLAUDE.md` is empty** → go ahead, the prompt handles everything.
- ⚠️ **You already have a mature custom cortex** (a custom CLAUDE.md > 30 lines you wrote yourself) → the prompt will **stop** at step 0 and offer 3 options (overwrite with backup / sandbox install / cancel). If you just want to cherry-pick a couple of ideas, read the repo files directly and do it manually — cleaner.

---

## 📋 Prompt to copy-paste into Claude Code

```
Install the "claude-brain" template for me from this GitHub repo:
https://github.com/levyan76/claude-brain

Exact procedure to follow (do not deviate):

STEP 0 — Existing cortex check (PROTECTION)
- Detect my OS (Windows / macOS / Linux) and adapt all following shell commands.
- Check whether ~/.claude/CLAUDE.md exists.
- Case A: the file doesn't exist → go to step 1.
- Case B: it exists and contains the "<TO FILL>" marker → it's an unfilled template
  from this same system, safe to overwrite. Go to step 1.
- Case C: it exists, > 30 lines, AND does NOT contain "<TO FILL>" → STOP. It's the
  user's custom cortex. Show them:
    "⚠️ You already have a personalized cortex (X lines). 3 options:
       (A) Overwrite everything with an automatic backup
       (B) Install in a sandbox at ~/.claude-brain-test/ (to try it without touching your real cortex)
       (C) Cancel"
  Wait for my explicit answer before continuing.
  - If (A) → back up ~/.claude/CLAUDE.md to ~/.claude/CLAUDE.md.backup-YYYY-MM-DD then go to step 1
  - If (B) → all following copies target ~/.claude-brain-test/ instead of ~/.claude/;
    tell me at the end how to activate the sandbox
  - If (C) → stop everything, touch nothing

STEP 1 — Clone
- Clone https://github.com/levyan76/claude-brain into a temp folder:
  - macOS / Linux: /tmp/claude-brain
  - Windows: $env:TEMP\claude-brain (PowerShell) or %TEMP%\claude-brain

STEP 2 — Prepare the destination
- Create ~/.claude/playbooks/ if it doesn't exist.

STEP 3 — Copy the cortex
- Copy CLAUDE.md from the clone → ~/.claude/CLAUDE.md
  (any backup from step 0 has already been made)

STEP 4 — Copy the playbooks WITH collision check
- List the files already in ~/.claude/playbooks/.
- List the files to copy from the clone (7 files expected):
    auto-introspection.md, defenses-idiot-proof.md, lessons-learned-format.md,
    nouveau-projet.md, posture-proactive.md, structure-memoire-projet.md, test-first-setup.md
- If name collisions:
    "⚠️ You already have these playbooks: <list>. 3 options:
       (1) Overwrite all
       (2) Skip collisions (keep yours, copy only the new ones)
       (3) Cancel the playbook copy"
  Wait for my answer before copying.
- If no collisions → copy the 7 playbooks normally.

STEP 5 — Verification
- Verify that ~/.claude/CLAUDE.md contains the "<TO FILL>" marker
  (otherwise the bootstrap won't trigger — that's an error signal).
- List the playbooks actually present in ~/.claude/playbooks/.

STEP 6 — Confirmation
- Confirm to me: "✅ Template installed. Cortex in place with <TO FILL> markers.
  Playbooks installed: <list>."

STEP 7 — Immediate bootstrap
- Chain IMMEDIATELY into the 10-question bootstrap interview described in the
  cortex ("🎤 Bootstrap interview" section).
- Ask them one at a time, wait for my answer between each, briefly explain
  why you're asking each question.

STEP 8 — Fill the cortex
- At the end of the bootstrap, fill the cortex with my answers (Edit/Write) and
  confirm I'm ready to use the system.
```

---

## Why this path is better for a beginner

- **No shell command to understand**: Claude detects your OS and uses the right paths
- **Existing cortex protection**: if you've already tinkered with your CLAUDE.md, the prompt stops and asks what to do
- **Playbook protection**: collision check before overwriting
- **Automatic backup**: on overwrite, your old cortex is saved with a date
- **Sandbox mode**: you can try it in `~/.claude-brain-test/` without touching your real setup
- **Chained bootstrap**: the 10-question interview starts right after — no restart needed
- **All in one session**: from clone to first question in ~30 seconds (if you're a beginner)

---

## If you prefer installing yourself

Go to [`GETTING-STARTED.md`](GETTING-STARTED.md) — manual method with copy-paste commands (macOS/Linux and Windows PowerShell separately).

---

## If something goes wrong

- Claude can't find `~/.claude/` → check that **Claude Code is installed** (not just the web app)
- The bootstrap doesn't trigger → check that `~/.claude/CLAUDE.md` still contains `<TO FILL>` (otherwise it thinks you're already configured)
- You want to redo the bootstrap → edit `~/.claude/CLAUDE.md` and put `<TO FILL>` back in the "My user" section, then say *"Hi Claude!"*
- You installed in sandbox and want to activate for real → copy `~/.claude-brain-test/CLAUDE.md` to `~/.claude/CLAUDE.md` (after backing up your old one), same for the playbooks
