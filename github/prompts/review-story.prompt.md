---
description: Review User Stories against the Definition of Ready and INVEST, propose rewrites or splits, then update ADO after confirmation.
agent: agent
argument-hint: Story IDs (e.g. 21, 24) or a tag (default demo-review)
---

# Review User Stories

**Input:** ${input:target:Story IDs, or a tag — default demo-review}

Follow `.github/copilot-instructions.md` for the Definition of Ready, INVEST checks, story standard, and golden rules.

## Step 1 — Fetch
- IDs → read each work item via the Azure DevOps MCP server.
- Tag (or empty input) → query User Stories in the project with that tag (default `demo-review`).
- Also read each story's **parent Feature** for context.

## Step 2 — Assess (do NOT write to ADO yet)

### Scorecard
| ID | Title | I | N | V | E | S | T | Verdict |
|---|---|---|---|---|---|---|---|---|
Use ✅ / ❌ per INVEST letter. **Verdict** = `Ready` · `Rewrite` · `Split`.

### Findings
For each story not Ready:
- **Problems** — specific and quoted (e.g. *AC "works correctly" has no pass/fail condition*). Max 4 bullets.
- **Proposed fix:**
  - `Rewrite` → full improved Title, Description, Acceptance Criteria (3–5 Given/When/Then).
  - `Split` → 2–4 smaller stories (full Title, Description, AC each). The **first** slice replaces the original story's content; the rest become **new sibling stories** under the same Feature.
- **Why it's better** — one line.

Then ask: **"Apply these changes in ADO? (yes / only #ID / change …)"** — and stop.

## Step 3 — Write (only after explicit confirmation)
Via MCP:
- **Rewrite** → update Title, Description, Acceptance Criteria on the existing item.
- **Split** → update the original with slice 1; create remaining slices as User Stories, children of the same Feature.
- Append tag `ai-generated` to every item you touched or created. **Keep all existing tags** (including `demo-review`).
- Add a comment on each updated item: `Reviewed by Copilot: <one-line summary of what changed and why>`.

Finish with `#ID — Title — Updated/Created` + links, and: *"Next: run /estimate-story on the new/updated stories"*.
