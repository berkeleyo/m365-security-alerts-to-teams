## Deploy: M365 Security Alerts → Teams

> Minimal steps to deploy the Logic App(s) that post Microsoft Entra/Microsoft 365 security signals into Teams.

> **What this posts**
>
> - **Risky users (updates)** — only new/changed items within the lookback window.  
> - **Risky sign-ins (updates)** — only new/changed items within the lookback window.  
>
> Low risk items are excluded by default (only **Medium** and **High**).

### Before you start
- Azure role: `Contributor` (resource group scope) and access to Microsoft Sentinel/Entra as needed.
- Teams channel where the connector will post.
- Parameters you’ll need: resource group name, location, Sentinel workspace name/ID (if used), Teams team/channel IDs.

<details>
<summary>Show exact prerequisites</summary>

- If using Graph or Teams connectors, ensure the Logic App’s **Managed Identity** has the minimal permissions you require.
- Optional: Create a dedicated resource group (e.g., `rg-sec-alerts-prod`).

</details>

### 1) Deploy the resources
1. **Open** the template in Azure Portal:  
   `Deployments → Create → Template (ARM/Bicep)`
2. **Upload** your template (`azuredeploy.bicep` or `azuredeploy.json`) and **Review + create**.
3. **Fill parameters**, for example:
   - `workspaceName`: `<your-sentinel>`
   - `logicAppName`: `la-alerts-to-teams`
   - `location`: `<your-region>`
4. **Create** the deployment and wait for `Succeeded`.

<details>
<summary>CLI alternative</summary>

```bash
az group create -n rg-sec-alerts-prod -l uksouth
az deployment group create \
  -g rg-sec-alerts-prod \
  -f ./samples/azuredeploy.json \
  -p workspaceName=<your-sentinel> logicAppName=la-alerts-to-teams location="UK South"
```
</details>

### 2) Connect to Teams
1. Go to **Logic Apps** → `la-alerts-to-teams` → **Workflows** → `notify-teams`.
2. Open the **Teams** connector action and **Sign in** / select the **Team** and **Channel**.
3. **Save** the workflow.

<details>
<summary>Find Team/Channel IDs</summary>

- In Teams: **…** next to the team → **Get link to team** (ID is in the URL).  
- Or use Microsoft Graph / CLI.

</details>

### 3) Wire to Sentinel / Risk feeds
1. In **Microsoft Sentinel** → **Analytics**, create/enable alert rules or queries that should trigger the playbook.
2. For each rule, set **Automated response** → **Add playbook** → select `notify-teams`.
3. **Save** and **Enable** the rule.

### 4) Test
1. **Run** the workflow with a sample payload (Logic App **Run Trigger**).
2. **Verify** a message appears in Teams.
3. If fields are missing, check **Runs history** → **Inputs/Outputs** for mapping errors.

### 5) Tuning (optional)
- **Rate-limit**: set trigger **concurrency = 1** to avoid bursty posts.  
- **Payload size**: cap list lengths (e.g., 10–20 items) to stay under the Teams ~28 KB limit.  
- **Logging**: enable diagnostic logs → Log Analytics → `WorkflowRuntime`.  
- **Security**: restrict Managed Identity/connector permissions to least privilege.

---

✅ **Done!** Your Logic App should now post **Risky users (updates)** and **Risky sign-ins (updates)** into your Teams channel.
