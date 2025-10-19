# M365 Security Alerts → Microsoft Teams (Managed Identity)

Logic Apps + Microsoft Sentinel playbooks that post **risky sign-ins / risky users** (and other alerts) to **Microsoft Teams** using a **Managed Identity** — no webhooks, no secrets.

- ✅ **Secure by design:** Graph API with Entra ID *Application permissions* granted to the Logic App’s managed identity  
- ✅ **Repeatable:** ARM/Bicep-style deployment via `az deployment group create`  
- ✅ **Clean repo:** No tenant/org names or secrets committed

---

## What you get

- **Playbook:** `notify-teams` – sends a rich message to a specific Team/Channel
- **Sample rule:** Sentinel analytics rule for Risky sign-ins / users (optional)
- **Docs:** step-by-step **deploy**, **operate**, and **troubleshoot**

> If you want to deploy right away, go to **[docs/DEPLOY.md](docs/DEPLOY.md)**.

---

## How it works (flow)

1. Microsoft Sentinel (or AAD Identity Protection) raises an alert/incident  
2. Sentinel automation rule triggers the **`notify-teams`** Logic App  
3. The Logic App uses its **Managed Identity** to call **Microsoft Graph**  
4. A formatted card is posted into your Teams channel

---

## Repository layout

