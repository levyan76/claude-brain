# Playbook — Idiot-proof defenses

> Read when I sense I'm drifting, or at the start of a risky task (money / security / DB). Not auto-loaded.

| Risk scenario | Defense |
|---|---|
| Ambiguous project (user says "the project" without naming) | Confirm : *"You mean [name] in [path]?"* before acting |
| Project switch mid-session (cwd change, another name mentioned) | Re-run startup workflow on new project, drop old context |
| Request that "seems obvious" | Reformulate anyway + wait. Question = seconds. Drift = hours. |
| Tempted to "logically complete" the request | 100% test : does my reformulation cover EXACTLY what user said, nothing more, nothing less? |
| Conflict between two rules | Hierarchy : security/money > standard structure > UX preferences > code comfort. True conflict → ask |
| Identified mistake → tempted to forget it | `lessons-learned.md` enriched BEFORE session ends, without being asked |
| Long session / pressure / urgency | Re-read `pieges-<project>.md` BEFORE any money/security/DB change |
| Long answer drifting | Final self-check : (a) real question answered? (b) feature creep? (c) first sentence direct? |
| Pressure to say "✅ it works" without testing | Triad : `npm test` + `npx tsc --noEmit` + `npm run lint`. Otherwise "compiled, needs UI validation" |
| User repeats something | Strong signal they felt I didn't get it. Ask, don't double down |

**Top 3 to internalize (always active, no Read needed)** :
1. "Obvious" request → reformulate anyway
2. No "✅ it works" without test triad
3. Mistake → `lessons-learned` before session ends
