# M365 Security Alerts → Teams

Posts Microsoft Entra / Microsoft 365 security signals to Microsoft Teams using **Azure Logic Apps** and **Managed Identity** (no webhooks or static secrets).

## What it posts
- **Risky users (updates)** — newly detected or updated risky users.
- **Risky sign-ins (updates)** — newly detected or updated risky sign-ins.

**Defaults**
- Filters to **Medium** and **High** risk.
- Sends **one concise Adaptive Card per run** (not one message per item).
- Supports **“no updates → no post.”**
- Caps list sizes to avoid the Teams ~28 KB payload limit.

## Quick links
- 🚀 **Deploy:** [`docs/DEPLOY.md`](docs/DEPLOY.md)  
- 🛠️ **Operate/Runbook:** [`docs/OPERATE.md`](docs/OPERATE.md)  
- 🔐 **Security model:** [`docs/SECURITY.md`](docs/SECURITY.md)  
- 🧪 **Example Adaptive Card (redacted):** [`docs/EXAMPLE_CARD.md`](docs/EXAMPLE_CARD.md)

## Why this approach?
- **Secure by default:** Managed Identity means no static secrets/webhooks.
- **Noise control:** single summarized card per run, concurrency cap = 1.
- **Simple operations:** idempotent deployment; easy rollback by disabling the workflow.

## License
[MIT](LICENSE)
