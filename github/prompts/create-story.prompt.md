---
description: Draft INVEST-ready User Stories from an ADO Feature ID or a one-line request, then create them in ADO after confirmation.
agent: agent
argument-hint: Feature ID (e.g. 12) or a one-line request (e.g. "Add satisfaction survey after chat")
---

# Create User Stories

**Input:** ${input:source:Feature ID or one-line request}

Follow `.github/copilot-instructions.md` for the story standard, field formatting, and golden rules.

## Step 1 — Understand the source
- **If the input is a number** → it's a Feature ID. Use the Azure DevOps MCP server to read it: title, description, acceptance criteria, parent Epic, and **existing child stories** (don't duplicate them).
- **If the input is text** → it's a request. Query the backlog via MCP for the Feature it best belongs to. If no Feature clearly fits, propose the best match and say so in **Assumptions**.

## Step 2 — Draft (do NOT write to ADO yet)
- From a Feature: draft **3–6 stories** that together cover the Feature's description (triggers, context, customer experience, constraints). Respect any "Out of scope".
- From a request: draft **1 story** (2 max if it clearly needs splitting).

Output exactly these sections:

### Summary
| # | Title | Role | Priority | Covers |
|---|---|---|---|---|

### Stories
For each story: **Title**, **Description** (As a… I want… so that…), **Acceptance Criteria** (3–5 Given/When/Then), **Priority**, **Parent:** `#ID Feature title`.

### Coverage check
One line per requirement in the source → which story covers it, or ⚠️ **Gap**.

### Assumptions
Bullet list (or "None").

Then ask: **"Create these N stories in ADO under #<FeatureID>? (yes / change …)"** — and stop.

## Step 3 — Write (only after explicit confirmation)
For each story, via MCP:
- Create a **User Story** as a **child of the Feature**
- Set Title, Description (HTML), Acceptance Criteria (HTML), Priority, Value Area = `Business`
- Do not set tags unless the user explicitly requests them
- Leave Story Points empty

Finish with a list: `#ID — Title` + link, and: *"Next: run /estimate-story #<FeatureID>"*.
