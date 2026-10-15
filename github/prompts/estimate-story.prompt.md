---
description: Estimate User Stories in Fibonacci points using anchor stories plus the live ADO baseline, then write points after confirmation.
agent: agent
argument-hint: Story IDs, a Feature ID, or empty for all unestimated ai-generated stories
---

# Estimate User Stories

**Input:** ${input:target:Story IDs, a Feature ID, or leave empty}

Follow `.github/copilot-instructions.md` for the estimation standard, anchors, and golden rules.

## Step 1 — Build the baseline
Via the Azure DevOps MCP server, query **User Stories in the project that already have Story Points**. Show it:

### Baseline
| ID | Title | Points |
|---|---|---|

## Step 2 — Select stories to estimate
- Story IDs → those stories.
- Feature ID → its child User Stories **without** Story Points.
- Empty → all User Stories tagged `ai-generated` **without** Story Points.
Read each story's Description and Acceptance Criteria. Skip any with no AC and flag them: *"Not estimable — run /review-story first"*.

## Step 3 — Estimate (do NOT write to ADO yet)

### Estimates
| ID | Title | Points | Confidence | Closest anchor | Closest baseline | Rationale |
|---|---|---|---|---|---|---|
- **Points:** 1, 2, 3, 5, 8 — or **13 ⚠️ split** (then propose a split instead of a final number).
- **Confidence:** High / Medium / Low.
- **Closest baseline:** `#ID (pts)` from Step 1.
- **Rationale:** ≤ 20 words — why bigger/smaller than the comparisons (integrations, auth, error paths, number of scenarios, unknowns).

### Totals
Total points · count by size · any stories flagged for split.

### Risks
Bullets for Low-confidence items: what question would raise confidence.

Then ask: **"Write these points to ADO? (yes / change #ID to N)"** — and stop.

## Step 4 — Write (only after explicit confirmation)
Via MCP, for each story:
- Set `Microsoft.VSTS.Scheduling.StoryPoints`
- Append tag `ai-generated` (keep existing tags)
- Add a comment: `Copilot estimate: N pts (confidence). Compared to anchor <n> and #ID (<pts>): <rationale>`

Finish with the updated list + links.
