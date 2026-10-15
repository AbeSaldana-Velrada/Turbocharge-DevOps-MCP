# Lab 05 — Estimate with `/estimate-story` + Wrap-up

> **Session:** Turbocharge Your Backlog · **Time:** ~20 min · **Level:** 200
> **Outcome:** Copilot estimates unpointed stories in Fibonacci, citing an anchor **and** a real baseline story for each, writes points after approval, and you reveal everything AI touched. Plus: reset for the next run.

## How Copilot estimates
| Source | Where it lives | Role |
|---|---|---|
| **Anchors** 1 / 3 / 8 | `.github/copilot-instructions.md` | Fixed scale |
| **Baseline** (6 pointed stories, tag `baseline`) | ADO, queried via MCP | Team context |
| **Rule** | 13 = must split | Keeps stories sprint-sized |

### Prerequisites
- [ ] Labs 03–04 complete (new + rewritten stories exist, no points)

---

## Exercise 1 — Estimate

**Task 1.1 — See the baseline**
```
/estimate-story
```
(empty input = all `ai-generated` stories without points)

**Task 1.2 — Read the output**
| Section | Check |
|---|---|
| Baseline table | 6 stories with points 1, 2, 3, 3, 5, 8 |
| Estimates | Each row cites **Closest anchor** + **Closest baseline `#ID (pts)`** |
| Confidence | Low items have a question under **Risks** |
| Totals | Any **13 ⚠️ split** flagged instead of a number |

**Task 1.3 — Challenge an estimate** (planning-poker moment)
```
Why is #<ID> a 5 and not a 3? Compare it directly to #<baseline ID>.
```
```
Change #<ID> to 3 — our team already has the Omnichannel connector built.
```

**Task 1.4 — Confirm**
```
yes, write the points
```
✅ **Checkpoint (in ADO):** Story Points set; each story has comment *Copilot estimate: N pts (confidence). Compared to anchor… and #ID…*

---

## Exercise 2 — The reveal

**Task 2.1 — Filter**
1. **Boards → Backlogs** → Stories level → **Filter** (funnel) → **Tags** = `ai-generated`.
2. Toggle **Parents** on in View options.

**Task 2.2 — Sprint planning bonus** (optional)
1. Open the **Planning** pane → drag the escalation stories into **Sprint 1**.
2. Watch the sprint's total points update — that's what the estimates are for.

✅ **Checkpoint:** One filtered view shows every story Copilot created, reviewed, or estimated.

---

## Exercise 3 — Copilot CLI cameo
```
copilot
```
```
In Azure DevOps project Copilot Studio Agent, list user stories tagged ai-generated with their story points, grouped by Feature. Do not use previously fetched data.
```
✅ **Checkpoint:** Same items + points as the ADO filter.

---

## Exercise 4 — Reset for the next run

**Task 4.1 — One-time setup**
1. Install PowerShell (Mac): `brew install --cask powershell`
2. ADO → **User settings → Personal access tokens → + New token** → scope **Work Items: Read, write, & manage** → 7-day expiry.

**Task 4.2 — Reset**
```powershell
pwsh
$env:ADO_PAT = '<your-pat>'
./scripts/reset-demo.ps1 -Org <your-org> -Project 'Copilot Studio Agent'
```
- Deletes all Epics/Features/Stories/Tasks (to Recycle Bin), re-imports `seed/backlog.csv` with parent links.
- Prints the new IDs → paste them into `Demo-Prompts.md`.

> ⚠️ IDs change on every reset. Update `Demo-Prompts.md` each time. Re-assign Sprint 1 stories if you use them.

✅ **Final checkpoint:** Backlog back to 3 Epics · 7 Features · 18 Stories, target Feature empty, 3 `demo-review` stories flawed again.

---

## Troubleshooting
| Symptom | Fix |
|---|---|
| Baseline table empty | Remote: confirm `X-MCP-Insiders: true` (WIQL tool). Or: `Use the backlog list tool and filter stories with Story Points.` |
| Estimates don't cite baseline | `Every estimate must cite a baseline #ID — redo.` |
| All estimates the same number | `Spread: justify each relative to anchors 1, 3, 8.` |
| Reset: 401 | PAT expired or wrong scope |
| Reset: "term not recognized" | Run inside `pwsh`, from the repo root |

## Knowledge check
1. Why cite a baseline story? → *Makes the estimate explainable and team-relative — like planning poker.*
2. What does 13 mean here? → *Too big — split before committing.*
3. Should the team accept AI estimates as final? → *No — they're a starting point; the team adjusts in refinement.*

---

## 🎉 You've completed the lab series
| Lab | You built |
|---|---|
| 00 | Entra demo identity + org |
| 01 | Agile project + seeded backlog |
| 02 | Copilot ↔ ADO via MCP (VS Code + CLI) |
| 03 | `/create-story` from Feature and prompt |
| 04 | `/review-story` with INVEST scorecard |
| 05 | `/estimate-story` with cited baselines + reset |

**Take it to your team:** copy `.github/` into your own repo, edit the anchors and Definition of Ready to match your team, and point `.vscode/mcp.json` at your org.
