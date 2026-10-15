# Lab 03 — Create User Stories with `/create-story`

> **Session:** Turbocharge Your Backlog · **Time:** ~20 min · **Level:** 200
> **Outcome:** Generate INVEST-ready stories two ways — from an ADO Feature (Path A) and from a one-line prompt (Path D) — with a human approval gate before anything is written.

## How it works
```
/create-story <Feature ID or text>
   │
   ├─ reads Feature via MCP (or finds best parent Feature)
   ├─ drafts stories using .github/copilot-instructions.md
   ├─ shows Summary · Stories · Coverage check · Assumptions
   └─ STOPS → you say "yes" → creates children in ADO, tag ai-generated
```

### Prerequisites
- [ ] Lab 02 complete — MCP running, slash commands visible
- [ ] IDs noted: **Live-agent escalation handoff** Feature, **Conversation analytics** Feature

---

## Exercise 1 — Path A: Create from an ADO Feature

**Task 1.1 — Inspect the source**
1. In ADO, open **Live-agent escalation handoff**. Read the description: triggers, context, customer experience, constraints, out of scope.
2. Confirm it has **0 child stories**.

**Task 1.2 — Draft**
```
/create-story #<escalation Feature ID>
```
Approve the read tool call(s).

**Task 1.3 — Inspect the draft** (don't confirm yet)
| Check | Look for |
|---|---|
| Format | Every Description = *As a <specific role>, I want…, so that…* |
| AC | 3–5 Given/When/Then per story, ≥1 negative case, numbers (10 s, 5 min, 8am–6pm CT) |
| Coverage check | Every trigger/constraint in the Feature mapped — any ⚠️ **Gap**? |
| Scope | Nothing about **voice** or **BPO** (out of scope) |

**Task 1.4 — Iterate once before confirming** (practice the human-in-the-loop)
```
Story 2: tighten the AC — add a scenario for when no human agent is available within 5 minutes.
```

**Task 1.5 — Confirm**
```
yes, create them
```

✅ **Checkpoint:** In ADO, the Feature now has 3–6 child stories, each tagged `ai-generated`, Story Points empty.

---

## Exercise 2 — Path D: Create from a one-line prompt

**Task 2.1 — Draft**
```
/create-story Add a satisfaction survey after the chat ends
```
**Task 2.2 — Check parent selection**
- Copilot should pick **Conversation analytics** (or explain its choice under **Assumptions**).
- Wrong parent? → `Use Feature #<ID> as the parent.`

**Task 2.3 — Confirm**
```
yes
```
✅ **Checkpoint:** 1 new story under the chosen Feature, tagged `ai-generated`.

---

## Exercise 3 — Stretch (optional)
| Try | Prompt |
|---|---|
| Different persona | `/create-story Let store managers see which customers were escalated today` |
| Pressure-test scope | `/create-story Add voice channel escalation` → should flag as out of scope for the Feature |
| Shorter output | Add `Max 3 stories, 3 AC each.` |

---

## What to point out to the audience
- **The Coverage check** → traceability from requirement to story, for free.
- **Assumptions** → AI states what it guessed instead of hiding it.
- **The stop** → nothing hits ADO without "yes".

## Troubleshooting
| Symptom | Fix |
|---|---|
| Writes to ADO without asking | Instructions not loaded — check **References**; confirm `.github/copilot-instructions.md` path |
| Stories created at root, not under Feature | `Re-link stories #A, #B as children of Feature #ID.` |
| Duplicates existing stories | `Ignore — first list the Feature's existing child stories, then draft only new ones.` |
| HTML tags visible as text in ADO | `Use HTML <p> and <br> in Description and Acceptance Criteria, not markdown.` |

## Knowledge check
1. Why draft-then-confirm? → *Human stays accountable; bad drafts never reach the backlog.*
2. What makes AC testable? → *Pass/fail outcome, concrete numbers, no "works correctly".*
3. Why tag `ai-generated`? → *Traceability + one filter shows everything AI touched.*

**Next:** Lab 04 — Review weak stories with `/review-story`.
