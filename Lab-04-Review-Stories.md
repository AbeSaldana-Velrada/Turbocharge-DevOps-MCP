# Lab 04 — Review Stories with `/review-story`

> **Session:** Turbocharge Your Backlog · **Time:** ~15 min · **Level:** 200
> **Outcome:** Copilot scores weak stories against INVEST and the Definition of Ready, proposes rewrites or splits, and applies them only after approval.

## The three planted defects
| Story | Defect | Expected verdict |
|---|---|---|
| Export conversation transcripts | AC = "works correctly" | **Rewrite** (T ❌) |
| Build the whole knowledge base integration | Epic-sized, 5 systems | **Split** (S ❌, E ❌) |
| Teams notifications | Title only — no role, no value, no AC | **Rewrite** (V ❌, E ❌, T ❌) |

### Prerequisites
- [ ] Lab 02 complete · seed imported (the 3 stories carry tag `demo-review`)

---

## Exercise 1 — Run the review

**Task 1.1 — Before**: open the 3 stories in ADO and screenshot them (your "before" slide).

**Task 1.2 — Review by tag**
```
/review-story demo-review
```

**Task 1.3 — Read the Scorecard**
| Check | Expect |
|---|---|
| INVEST columns | ❌ in the letters from the table above |
| Findings | Problems are **quoted** from the story, not generic |
| Split proposal | 2–4 slices, each ≤ 8 pts-sized (e.g. SharePoint · website · PDF manuals · refresh schedule) |
| Rewrite of "Teams notifications" | Specific role + benefit — Copilot had to **infer** intent → check **Assumptions** |

✅ **Checkpoint:** 3 verdicts: Rewrite · Split · Rewrite.

---

## Exercise 2 — Steer, then apply

**Task 2.1 — Challenge one finding** (shows it's a conversation, not a black box)
```
For the Teams notifications story, the user is a store employee who needs to be alerted when a customer escalates in their region. Redraft.
```

**Task 2.2 — Apply selectively**
```
yes, apply all
```
— or partial: `only apply #<ID>`

✅ **Checkpoint (in ADO):**
- Rewritten stories have new Description + AC, tag `ai-generated`, and **still** `demo-review`
- The big story now holds slice 1; other slices are **new siblings** under the same Feature
- Each touched item has a comment: *Reviewed by Copilot: …*

---

## Exercise 3 — Stretch (optional)
| Try | Prompt |
|---|---|
| Review AI's own work | `/review-story` on 2 stories created in Lab 03 — does it pass its own DoR? |
| Review a whole Feature | `/review-story` with all child story IDs of a Feature |
| Strict mode | Add `Be strict: any missing negative scenario = Rewrite.` |

---

## What to point out to the audience
- Same standard for **creating** and **reviewing** → consistency across BAs and POs.
- **Split** is the hardest thing for humans in refinement — AI proposes slices in seconds.
- The comment trail = audit history on every item.

## Troubleshooting
| Symptom | Fix |
|---|---|
| "No stories found" for tag | Use IDs instead: `/review-story #A, #B, #C` |
| Removed `demo-review` tag | `Restore tag demo-review on #A, #B, #C` — reset script relies on it |
| Generic findings ("add more detail") | `Quote the exact text that fails each INVEST letter.` |

## Knowledge check
1. Which INVEST letter does "AC: works correctly" fail? → *Testable.*
2. Why keep the original ID when splitting? → *Preserves history, links, and comments.*
3. What should a reviewer do with **Assumptions**? → *Confirm or correct them before applying.*

**Next:** Lab 05 — Estimate with `/estimate-story`.
