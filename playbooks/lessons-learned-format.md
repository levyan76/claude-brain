# Playbook — Lessons learned (the learning mechanism)

> Read when creating or enriching a `lessons-learned.md` file. **This is the central mechanism that makes the whole system self-improving.**

## Why it matters

Without `lessons-learned.md`, every session restarts from zero :
- I repeat the same mistakes
- I miss patterns that already worked well
- The user has to re-correct me on the same things
- Knowledge accumulated during a session is lost at the end

With it, knowledge **compounds in both directions** :
- I learn from mistakes (don't repeat X)
- I learn from successes (repeat Y when seeing Z)

It's the difference between an assistant that resets every session and a partner that **actually learns** — not just through fear of doing wrong, but also through consolidating what works.

---

## Two entry types in TWO separate files

| File | Format | Marker | What |
|---|---|---|---|
| `lessons-learned.md` | Format A | ❌ | Errors / mistakes / identified traps |
| `wins.md` | Format B | ✅ | Successes / patterns that work / user validations |

Why two files (not one mixed) :
- Each file stays **lean** → readable even after 2 years of accumulation
- At session start : I re-read the latest 3-5 entries of EACH → **guaranteed balanced signal**, not diluted
- Targeted search : *"what did we miss on X?"* vs *"what works well for Y?"* → 1 dedicated file, no filtering needed
- Separate volume reflects a useful reality : 50 successes / 10 errors = healthy project

### Format A — Error / mistake

```markdown
## YYYY-MM-DD — ❌ Short, precise title of the problem

- **Context** : Where / when / why I was working on this. Project state at that moment.
- **Error / Symptom** : What went wrong. Concrete, measurable.
- **Root cause** : WHY it went wrong. Not "I made a mistake" — the real technical or logical reason.
- **Solution** : What fixed it. Code, command, decision, refactor.
- **Takeaway** : The general lesson that can apply elsewhere. This is what I'll re-read in 3 months.
```

### Format B — Success / discovery / pattern that works

```markdown
## YYYY-MM-DD — ✅ Short title of what worked

- **Context** : Where / when / why I tried it. What problem I was trying to solve.
- **Approach** : What I did concretely. Not vague — the action sequence or precise decision.
- **Why it worked** : The mechanism. Not just "it worked" — why this approach was right here.
- **When to reuse** : Validity conditions. When this approach applies vs when it doesn't.
- **Takeaway** : The transferable pattern. How I'll recognize the next occasion to use this.
```

---

## Why two formats (not one generic)

The questions to ask aren't the same :
- For an **error** : "why did I fall into it" + "how to avoid"
- For a **success** : "why did it work" + "when to do it again"

A unified format would force mislabeling fields. Two explicit formats = clarity.

---

## ⚠️ Critical discipline — lessons are NEVER universal

**A poorly formulated lesson becomes a barrier in other contexts.**

Concrete example : if I write *"always prefer short code to expressive code"* as a general lesson, I'll under-document in a context where clarity > concision. The lesson becomes an anti-pattern outside its original context.

**Rule for every entry (A or B)** :

1. **The "When to reuse" field (Format B) or implicit (Format A) must always be present and precise** — not just "when useful", but the concrete conditions.

2. **Always also document the INVERSE context** : when this lesson does NOT apply. If you can't name at least one case where it's false → either the lesson is trivial (don't write it), or you haven't thought enough.

3. **Phrase the "Takeaway" as a conditional pattern, not a universal maxim** :
   - ❌ *"Always do X"* → potential barrier
   - ✅ *"When <condition>, do X rather than Y"* → cleanly reusable

**Self-check at every writing** : *"If the user asks me tomorrow about the INVERSE context, will this lesson block me?"* If yes → reformulate.

---

## Good vs bad examples

### ❌ Error — bad example

```markdown
## 2026-06-15 — ❌ Pricing bug
Had a bug with pricing. Fixed by modifying the calculation.
Takeaway : be careful with pricing.
```

Why bad : no root cause, no transferable lesson.

### ❌ Error — good example

```markdown
## 2026-06-15 — ❌ Margin double-applied in the pricing module

- **Context** : Refactoring the pricing module to add volume discount. `calculatePrice()` in `lib/pricing/core.ts`.
- **Error / Symptom** : Final prices were 21% above expected. Unit tests passed anyway.
- **Root cause** : Gross margin was already applied in `getBasePrice()` (for 2 months) AND I re-applied it in `calculatePrice()`. Tests passed because they used the function's output as ground truth.
- **Solution** : Renamed `getBasePrice()` → `getCostPrice()` to clarify. Added a test that compares final price to a manually-derived expected value.
- **Takeaway** : When a function is named "getXxx", check its code for what it actually computes. Names drift from content. Add a test that derives the expected value by hand.
```

### ✅ Success — bad example

```markdown
## 2026-06-20 — ✅ Brain model
Good idea to organize like a brain. Do again.
```

Why bad : no validity conditions, no mechanism explained. Unusable in 3 months.

### ✅ Success — good example

```markdown
## 2026-06-24 — ✅ Asking Claude "what's slowing you down?" instead of installing more skills/MCPs

- **Context** : User found Claude less performant. Classic temptation : install more skills/MCPs to "improve".
- **Approach** : Asked Claude directly *"what's slowing you down in MY workflow?"* + real measurement (wc -l, wc -c on auto-loaded files). Diagnosis : CLAUDE.md at 414 lines = 6k wasted tokens/message.
- **Why it worked** : Claude knows its own bottlenecks better than any generic tutorial. Measuring = moving from feeling to numbers = grounded decisions. Skills/MCPs add, they don't cure bloat.
- **When to reuse** : Before ANY addition of skill/MCP/plugin. When user signals perf drop. When a memory file exceeds the auto-introspection playbook thresholds.
- **Takeaway** : "Measure + listen to what's already there" > "add stuff hoping". If the intuitive solution is "install X", challenge first by measuring the existing.
```

---

## When to write an entry

**Always** :
- ❌ Identified mistake (mine or one we fixed together)
- ❌ Non-trivial problem solved (≥30 min of fumbling)
- ❌ Surprising pattern discovered (library not behaving as expected)
- ❌ User corrects me 2× on the same kind of thing
- ✅ Non-obvious success (unconventional approach that worked well)
- ✅ Explicit user validation of a decision (*"great idea"*, *"exactly"*) — strong signal to consolidate
- ✅ Pattern we invented together that becomes reusable

**Never** :
- Trivial typos / renames
- Syntax errors fixed in 5 seconds
- Stuff already in `pieges-<project>.md` (then reinforce the trap, not write a lesson)
- Obvious successes ("code compiled on first try") = not a lesson

**Cadence** : enrich BEFORE session ends, not tomorrow morning. Otherwise I lose the detail.

---

## Project vs meta hierarchy

Both files exist at two levels :

| Level | Files | For what |
|---|---|---|
| Per project : `~/.claude/projects/<project>/memory/` | `lessons-learned.md` + `wins.md` | Project-specific (bugs, gotchas, patterns that work in this context) |
| Meta : `~/.claude/projects/<meta-project>/memory/` | `lessons-learned.md` + `wins.md` | About my own functioning (workflow, memory, communication, collaboration methods) |

Rule : if the lesson applies to 1 project only → project. If it applies to how I work in general → meta.

---

## Connection to other memories

- If the error reveals a **recurring trap** → also add to `pieges-<project>.md` (1-line summary)
- If the error implies a **new reflex** → also add a trigger in the cortex
- If the lesson is applicable to **other projects** → mention *"ported to [other project]"* (cross-project capitalization)
- If a success becomes a **systematic pattern** → elevate it to a new playbook if applied 3+ times

---

## Auto-trigger

Without being asked, as soon as :
- Mistake identified → Format A entry in `lessons-learned.md` BEFORE continuing
- Non-trivial success validated → Format B entry in `wins.md` BEFORE session ends
- User says *"great idea"* / *"keep going"* / *"exactly"* → orientation signal → Format B entry in `wins.md` to consolidate
- User says *"we already had this problem"* → check `lessons-learned.md`, follow documented solution
- User says *"we did something that works for this"* → check `wins.md`, reuse the pattern

This is what makes the system **truly self-improving**, not just "neatly organized".
