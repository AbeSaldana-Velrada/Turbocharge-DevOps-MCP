# Lab 01 — Build an Agile Backlog in Azure DevOps

> **Session:** Turbocharge Your Backlog (GitHub Copilot + ADO MCP)
> **Time:** ~30 min · **Level:** 200 · **Outcome:** A demo-ready Agile project with Epic → Feature → User Story hierarchy, sprints, and a story-point baseline.

---

## Lab overview

| Exercise | What you build | Time |
|---|---|---|
| 1 | Agile project | 5 min |
| 2 | Backlog levels + sprints + areas | 10 min |
| 3 | Epic → Feature → Story hierarchy | 10 min |
| 4 | Verify + estimation baseline | 5 min |

### Prerequisites
- [ ] **Lab 00 complete** — Entra-backed org, signed in as `demo@<tenant>.onmicrosoft.com` (Project Collection Administrator)
- [ ] Private browser window signed in as the **demo user** (same identity the remote MCP server uses in Lab 02)
- [ ] Demo scenario: *Copilot Studio customer-service agent*

### Key concepts (read once)
| Term | Meaning |
|---|---|
| **Organization** | Top-level container (`dev.azure.com/<org>`). Billing + users live here. |
| **Project** | Holds Boards, Repos, Pipelines. Your backlog lives here. |
| **Process** | Template that defines work item types. **Agile** = Epic → Feature → User Story → Task. |
| **Story Points** | Agile's estimate field (Scrum uses *Effort* on PBIs instead). |
| **Iteration path** | Sprint. Must be created at project level **and** selected by the team. |
| **Area path** | Functional slice (Agent, Integrations…). Used for filtering/ownership. |
| **Environment (Pipelines)** | Deployment target, **not** a backlog container. Not used in this lab. |

> 💡 The process is chosen at project creation. Changing it later means migrating to an inherited process — pick **Agile** now.

---

## Exercise 1 — Create the Agile project

**Task 1.1 — New project**
1. Open `https://dev.azure.com/<your-org>` → **+ New project**.
2. Fill in:
   - **Project name:** `Copilot Studio Agent`
   - **Description:** `Demo backlog for Turbocharge Your Backlog`
   - **Visibility:** `Private`
3. Expand **Advanced** → **Version control:** `Git` · **Work item process:** `Agile`.
4. Click **Create**.

✅ **Checkpoint:** Project summary page loads. **Boards** appears in the left nav.

---

## Exercise 2 — Configure backlog levels, sprints, areas

**Task 2.1 — Enable the Epics backlog**
1. **Project settings** (bottom-left) → **Boards → Team configuration**.
2. **Backlogs** tab → check **Epics** (plus Features, Stories) → **Save**.

**Task 2.2 — Create sprints (project level)**
1. **Project settings → Boards → Project configuration → Iterations**.
2. Select the root → **New child** ×4:

| Name | Start | End |
|---|---|---|
| Sprint 1 | next Monday | +2 weeks |
| Sprint 2 | +2 weeks | +4 weeks |
| Sprint 3 | +4 weeks | +6 weeks |
| Sprint 4 | +6 weeks | +8 weeks |

**Task 2.3 — Assign sprints to the team** ⚠️ *most-missed step*
1. **Team configuration → Iterations** → **+ Select iteration(s)**.
2. Add Sprint 1–4.

**Task 2.4 — Area paths (optional, recommended)**
1. **Project configuration → Areas** → add children: `Agent`, `Integrations`, `Admin`.
2. **Team configuration → Areas** → include sub-areas.

✅ **Checkpoint:** **Boards → Sprints** shows Sprint 1 with dates. Backlog level dropdown lists **Epics**.

---

## Exercise 3 — Build the hierarchy (top-down)

**Task 3.1 — Epics**
1. **Boards → Backlogs** → level dropdown (top-right) → **Epics**.
2. **+ New Work Item** → enter title → **Add to top**. Create 3 epics: `Agent Core`, `Integrations`, `Admin & Analytics`.

> 🎯 **Demo-specific items** (detail in `Demo-Plan-Decisions.md`):
> - Feature **Live-agent escalation handoff** — rich description, **zero child stories** (Copilot fills it live)
> - 3 weak stories tagged `demo-review`: vague AC · too big · no "so that"

**Task 3.2 — Features under each Epic**
1. On an epic row, click **+** (Add child) → **Feature**. 2–3 per epic.

**Task 3.3 — User Stories under each Feature**
1. Switch level to **Features** → **+** on a feature row → **User Story**. 2–4 per feature.
2. Open each story and fill **Description** and **Acceptance Criteria** (Given/When/Then).

> ⚡ **Fast path (recommended):** Import `seed/backlog.csv` via **Boards → Queries → Import work items** → **Import** → **Save items**. Creates all 3 Epics, 7 Features, 18 Stories with parent links, 6 pointed baseline stories, and the 3 `demo-review` stories in one go. Hierarchy comes from the `Title 1 / Title 2 / Title 3` columns.

✅ **Checkpoint:** Every story has a parent feature; every feature has a parent epic.

---

## Exercise 4 — Verify and set the estimation baseline

**Task 4.1 — Verify hierarchy**
1. Epics level → **View options** → toggle **Parents** on → expand all.
2. Confirm nesting: Epic → Feature → User Story.

**Task 4.2 — Seed estimates**
1. Set **Story Points** on 3–5 stories (use 1, 2, 3, 5, 8).
2. Leave the rest **blank** — Copilot estimates them live in the demo.
3. Drag 2–3 estimated stories into **Sprint 1** (Planning pane).

**Task 4.3 — Demo prep**
- Bookmark: Backlog (Epics level), Sprint 1 board, a single story URL.
- Note your **org name** and **project name** — needed for the MCP config in Lab 02.

✅ **Final checkpoint:** Backlog shows a 3-level tree, Sprint 1 has work, and ≥50% of stories are unestimated.

---

## Troubleshooting

| Symptom | Fix |
|---|---|
| No **Epics** option in backlog | Task 2.1 — enable Epics in Team configuration |
| Sprints missing from Planning pane | Task 2.3 — select iterations for the team |
| Work items "disappear" after creating | Item's Area/Iteration isn't in the team's settings → include it |
| Can't add Feature under Epic | Wrong level selected; switch to Epics, use row **+** |
| No Story Points field | Project uses Scrum/Basic → recreate with Agile |

---

## Knowledge check
1. Why Agile over Scrum for this demo? → *User Story + Story Points, matching the estimation story.*
2. You created sprints but the board is empty of them — why? → *Not selected in Team configuration → Iterations.*
3. Is an ADO "Environment" where the backlog lives? → *No — Environments are Pipelines deployment targets.*

---

**Next:** Lab 02 — Connect GitHub Copilot to ADO with the Azure DevOps MCP server.
