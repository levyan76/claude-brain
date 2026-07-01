# Reddit post draft — r/ClaudeAI

**Suggested title** :
> I cut my Claude Code CLAUDE.md from 414 → 99 lines (−77% tokens/message) by treating it like a brain. Template inside.

**Body** :

After 3 months of solo dev with Claude Code on several SaaS projects, my `~/.claude/CLAUDE.md` had bloated to **414 lines / ~6k tokens loaded on every single message**. Claude felt slower. My token bill went up. I was about to install yet more skills and MCPs thinking that would help.

Instead, I asked Claude itself : *"what's slowing you down in MY workflow specifically?"* The answer was clear once we measured : my CLAUDE.md was being treated as a knowledge dump instead of as working memory.

We redesigned it using the human brain as a model :

- **Cortex** (CLAUDE.md, always loaded) → only identity, hard reflexes, and a **trigger map** (the hippocampus)
- **Procedural memory** (`~/.claude/playbooks/`) → workflows like "new project setup", "test-first config", read **only when triggered**
- **Episodic/semantic** (`~/.claude/projects/<slug>/memory/`) → per-project state, traps, lessons

The trigger map is the key innovation. A single table inside the cortex that routes Claude to the right playbook based on context :

```
| If I see / if the action is...        | Immediate action |
| New project / "let's launch X"        | Read playbooks/nouveau-projet.md BEFORE acting |
| Money/security/DB-critical task       | Read pieges-<project>.md BEFORE writing a line |
| About to say "✅ it works"            | Run triad: npm test + tsc + lint |
| User says "less efficient / too slow" | Read playbooks/auto-introspection.md → measure, propose fixes |
| ...                                   | ... |
```

**Measured impact** : 414 → 99 lines on the cortex. Roughly 4-5k fewer tokens per message. Investment (one ~50k-token meta session) paid back after ~10 messages.

**The compounding piece — `lessons-learned.md` + `wins.md`** : the most underrated part of this system. Each project's memory has TWO learning files :
- `lessons-learned.md` (❌) — errors, mistakes, traps. Mandatory format : Context / Error / Root cause / Solution / Takeaway.
- `wins.md` (✅) — successes, patterns that work, explicit user validations ("great idea"). Mandatory format : Context / Approach / Why it worked / When to reuse / Takeaway.

Every non-trivial event gets an entry BEFORE session ends. Read at next session startup. Why two files : keeps each lean over time + balanced signal at startup (5 latest of each, not diluted). Result : Claude stops repeating mistakes AND consolidates what works — knowledge compounds in both directions across months. The difference between "AI assistant" and "team member who's been with you for 6 months".

**Bonus reflex I added** : at every significant session end, Claude **automatically** measures its own architecture (file sizes, redundancy, dead triggers) and proposes 1-3 optimizations. Turns Claude into a co-designer of its own setup instead of a passive tool.

I anonymized my actual files into a copy-paste template :
**[GitHub Gist link goes here]**

Contains :
- `CLAUDE.md` (cortex, 99 lines)
- 6 playbooks (auto-introspection, nouveau-projet, test-first-setup, structure-memoire-projet, posture-proactive, defenses-idiot-proof)
- README explaining the brain model

**Honest caveats** :
- Not a research paper. Letta (ex-MemGPT), mem0, CoALA already formalize this more rigorously. This is the **pragmatic version** that runs in your terminal today, no framework install.
- Best for solo devs with several side projects.
- Requires discipline : every time you're tempted to add 30 lines to CLAUDE.md, ask first if it belongs in a playbook with a trigger.

Would love feedback from anyone trying it — and especially from those who have a different memory architecture working for them. The whole point is to compare patterns and improve.

---

## Optional comments to seed in replies

**If someone asks how the trigger map works in practice :**
> The model reads the cortex on every message — including the trigger map. When it sees text matching a trigger (e.g. "new project"), the table tells it which playbook to Read before acting. No fancy embedding/RAG — just markdown matching, but it works because Claude actually reads its own instructions.

**If someone asks about cost :**
> ~50k tokens to do the meta-session (measuring, designing, writing playbooks). Saves ~4-5k tokens per message after. Break-even at ~10 messages. For a heavy user, that's day 1.

**If someone asks why not just use Claude Skills :**
> Anthropic Skills are great but more heavyweight and global. Playbooks are project-local files you fully control, version, and can edit in 5 seconds. Different trade-offs.
