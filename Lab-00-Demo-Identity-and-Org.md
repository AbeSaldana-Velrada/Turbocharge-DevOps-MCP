# Lab 00 — Create an Entra-Backed Demo Identity & ADO Org

> **Session:** Turbocharge Your Backlog (GitHub Copilot + ADO MCP)
> **Time:** ~20 min · **Level:** 100 · **Outcome:** A clean `demo@<tenant>.onmicrosoft.com` identity and an Azure DevOps org connected to Microsoft Entra ID — required for the **remote** ADO MCP server.

---

## Why this lab exists
| Org type | Remote MCP (`mcp.dev.azure.com`) | Local MCP (`npx @azure-devops/mcp`) |
|---|---|---|
| **Entra-backed** (work/school account) | ✅ | ✅ |
| **Personal MSA** (@outlook, @gmail…) | ❌ not supported | ✅ |

On stage we use **remote** → the org **must** be Entra-backed. Skip this lab only if you're doing **Track B (local server)**.

### Prerequisites
- [ ] Personal email + phone for Azure sign-up verification (a card may be requested for identity check; free tier is not charged)
- [ ] A **private/incognito** browser window — keeps your other Microsoft sign-ins out of the demo

---

## Exercise 1 — Create the Entra tenant

**Task 1.1 — Free Azure account**
1. Private window → `https://azure.microsoft.com/free` → **Start free**.
2. Complete sign-up. This creates a tenant like `<something>.onmicrosoft.com`.

**Task 1.2 — Find your tenant domain**
1. Go to `https://entra.microsoft.com` → **Overview**.
2. Copy the **Primary domain** (e.g. `abedemo.onmicrosoft.com`).

✅ **Checkpoint:** You can see a Tenant ID and Primary domain.

---

## Exercise 2 — Create the demo user

**Task 2.1 — New user**
1. Entra admin center → **Users → All users → + New user → Create new user**.
2. Fill in:
   - **User principal name:** `demo@<your-domain>.onmicrosoft.com`
   - **Display name:** `Demo Presenter`
   - **Password:** auto-generate → **copy it**
3. **Review + create**.

> 💡 No Azure role needed. ADO permissions come from the org you create next.

✅ **Checkpoint:** `demo@…` appears under All users.

---

## Exercise 3 — Create the ADO org as the demo user

**Task 3.1 — First sign-in**
1. **New** private window → `https://dev.azure.com` → **Sign in**.
2. Use `demo@<your-domain>.onmicrosoft.com` + temp password → set a new password.

**Task 3.2 — Create the org**
1. **Create new organization** → name it (e.g. `abe-backlog-demo`) → choose region → **Continue**.
2. Skip/close the "create project" prompt — Lab 01 handles that.

**Task 3.3 — Verify Entra connection** ⚠️ *gate check*
1. **Organization settings** (bottom-left) → **Microsoft Entra**.
2. Must show your tenant (`<your-domain>.onmicrosoft.com`) as **connected**.

✅ **Final checkpoint:** Org URL `https://dev.azure.com/<org>` works and Microsoft Entra page shows your tenant.

---

## Troubleshooting
| Symptom | Fix |
|---|---|
| Org shows "not connected to Entra" | You signed in with a personal account. Sign out, private window, sign in as `demo@…` |
| Wrong account auto-signs in | Close all private windows, open a fresh one |
| "Need admin approval" on later MCP sign-in | You're the tenant admin — sign in with the admin account once and consent for the org |
| Forgot demo password | Entra admin center → Users → demo → **Reset password** |

---

## Write these down (needed in Lab 02)
| Item | Value |
|---|---|
| Tenant domain | `__________.onmicrosoft.com` |
| Demo user | `demo@__________.onmicrosoft.com` |
| ADO org name | `__________` |
| Remote MCP URL | `https://mcp.dev.azure.com/<org>` |

> 🔑 GitHub Copilot signs in with your **GitHub** account (separate). The ADO MCP server signs in with the **Entra demo user**. Two identities — that's normal.

**Next:** Lab 01 — Build an Agile Backlog in Azure DevOps.
