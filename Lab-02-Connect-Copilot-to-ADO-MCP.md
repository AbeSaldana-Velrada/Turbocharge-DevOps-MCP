# Lab 02 — Connect GitHub Copilot to Azure DevOps with MCP

> **Session:** Turbocharge Your Backlog (GitHub Copilot + ADO MCP)
> **Time:** ~25 min · **Level:** 200 · **Outcome:** VS Code Copilot (Agent mode) and Copilot CLI can read and write your ADO backlog, and the `/create-story`, `/review-story`, `/estimate-story` commands are loaded.

---

## Pick your track
| | **Track A — Remote** (recommended, used on stage) | **Track B — Local** |
|---|---|---|
| Org type | Entra-backed (Lab 00) | Any, incl. personal Microsoft account |
| Install | Nothing | Node.js 20+ |
| Transport | HTTP → `https://mcp.dev.azure.com/<org>` | stdio → `npx @azure-devops/mcp` |
| Sign-in | Microsoft Entra (browser) | Interactive Microsoft sign-in (default) |
| Config file | `.vscode/mcp.json` | `.vscode/mcp.local.json` → rename to `mcp.json` |

> ⚠️ Never run both servers at once — duplicate tools confuse the agent.

### Prerequisites
- [ ] Lab 00 (Track A) and Lab 01 done — seed backlog imported
- [ ] **VS Code** (latest) + **GitHub Copilot** and **Copilot Chat** extensions, signed in with your **GitHub** account (paid plan recommended for rehearsals — Free tier has monthly chat limits)
- [ ] **GitHub Copilot CLI** installed (`npm install -g @github/copilot`) — for Exercise 5
- [ ] Track B only: **Node.js 20+** (`node -v`)

### Two identities — this is normal
| Tool | Signs in with |
|---|---|
| GitHub Copilot (VS Code / CLI) | Your **GitHub** account |
| ADO MCP server | Your **ADO identity** (`demo@<tenant>.onmicrosoft.com` on Track A) |

---

## Exercise 1 — Prepare the workspace

**Task 1.1 — Folder names** (Copilot only reads dot-folders)
1. Open the `DevOps ADO` folder in VS Code (**File → Open Folder**).
2. If you have a folder named `github`, rename it to **`.github`**. If you have `vscode`, rename to **`.vscode`**.
3. Confirm this structure:
```
.github/copilot-instructions.md
.github/prompts/create-story.prompt.md
.github/prompts/review-story.prompt.md
.github/prompts/estimate-story.prompt.md
.vscode/mcp.json            ← remote (Track A)
.vscode/mcp.local.json      ← local  (Track B)
seed/backlog.csv
```

**Task 1.2 — Set your names**
1. `.github/copilot-instructions.md` → replace `<your-org>` with your org name.
2. Confirm **Project:** matches your ADO project exactly (`Copilot Studio Agent`).

✅ **Checkpoint:** Explorer shows `.github` and `.vscode` folders.

---

## Exercise 2 — Track A: Remote server

**Task 2.1 — Configure**
1. Open `.vscode/mcp.json` → replace `<your-org>` in the URL.
2. Review the headers:
   - `X-MCP-Toolsets: wit,work` → only work-item + iteration tools (fewer tools = sharper agent)
   - `X-MCP-Insiders: true` → unlocks `wit_query_by_wiql`, used by `/estimate-story` to find pointed stories

**Task 2.2 — Start + sign in**
1. Click **Start** above `ado-remote-mcp` in `mcp.json`.
2. Browser opens → sign in as **`demo@<tenant>.onmicrosoft.com`** → accept consent.
3. Status above the server changes to **Running · N tools**.

✅ **Checkpoint:** Running with tools listed. Skip to Exercise 4.

---

## Exercise 3 — Track B: Local server

**Task 3.1 — Swap configs**
1. Rename `.vscode/mcp.json` → `mcp.remote.json` (park it).
2. Rename `.vscode/mcp.local.json` → **`mcp.json`**.
3. Note `-d core work work-items` in `args` → loads only the domains we need.

**Task 3.2 — Start + sign in**
1. Click **Start** above `ado` → enter your **org name** when prompted.
2. First tool call opens a Microsoft sign-in → use the account that owns the org.

> 💡 Auth fails? Alternative: `az login`, then add `"--authentication", "azcli"` to `args`.

✅ **Checkpoint:** `ado` shows **Running · N tools**.

---

## Exercise 4 — Verify in Copilot Chat

**Task 4.1 — Agent mode + tools**
1. Open Copilot Chat → mode dropdown → **Agent**.
2. Click the **Tools** (🔧) icon → confirm the ADO server is checked.

**Task 4.2 — Smoke tests** (type each, approve tool calls with **Allow**)
```
List the projects in my Azure DevOps organization.
```
```
In project Copilot Studio Agent, list the Features with their IDs and parent Epic.
```
```
Show the User Stories tagged demo-review with their IDs.
```

**Task 4.3 — Confirm instructions + commands loaded**
1. In any reply, expand **References** → `copilot-instructions.md` should be listed.
2. Type `/` in chat → `create-story`, `review-story`, `estimate-story` appear.
   - Missing? Settings → search **prompt files** → enable `chat.promptFiles`, reload window.

**Task 4.4 — Record your IDs** in `Demo-Prompts.md` (target Feature, Conversation analytics Feature, 3 `demo-review` stories).

> 🎤 **Stage tip:** For rehearsals choose **Allow in this Workspace** on tool prompts. On stage, use plain **Allow** for the first call so the audience sees the approval card — then allow for the session.

✅ **Checkpoint:** All 3 smoke tests return real data; 3 slash commands visible.

---

## Exercise 5 — Copilot CLI (the 60-second cameo)

**Task 5.1 — Add the server**
1. Terminal → run `copilot` → sign in with GitHub if asked.
2. Type `/mcp add` and follow the prompts **or** copy the matching file into `~/.copilot/mcp-config.json`:
   - Track A → `copilot-cli/mcp-config.remote.json`
   - Track B → `copilot-cli/mcp-config.local.json`
3. Replace `<your-org>`, then restart `copilot`.

**Task 5.2 — Verify**
```
In Azure DevOps project Copilot Studio Agent, list the Epics and count the User Stories under each.
```
✅ **Checkpoint:** CLI returns the same 3 Epics you see in VS Code. (Copilot CLI also reads `.github/copilot-instructions.md` when started from this folder.)

---

## Troubleshooting
| Symptom | Fix |
|---|---|
| Remote: sign-in loops / "not supported" | Org is MSA-backed → finish Lab 00 or use Track B |
| Remote: "need admin approval" | Sign in once as the tenant admin and grant consent for the org |
| Server running but 0 tools | Check `X-MCP-Toolsets` spelling (`wit,work`); restart server |
| Copilot answers without calling ADO | Not in **Agent** mode, or server unchecked in Tools |
| Stale results | Add "Do not use previously fetched data." to the prompt |
| Local: `npx` not found | Install Node.js 20+, restart VS Code |
| Both servers listed | Stop one; keep only one `mcp.json` |
| Slash commands missing | Folder must be `.github/prompts/`, files end in `.prompt.md` |

## Knowledge check
1. Why does stage use remote? → *No install, Microsoft-managed updates, Entra auth.*
2. What does `X-MCP-Toolsets` buy you? → *Fewer tools → faster, more accurate tool selection.*
3. Which identity does the MCP server use? → *Your ADO identity, not your GitHub account.*

**Next:** Lab 03 — Create stories with `/create-story`.
