## Deploy: M365 Security Alerts → Teams

> Minimal steps to deploy the Logic App(s) that post Microsoft 365/Sentinel alerts into Teams.

### Before you start
- Azure role: `Contributor` (resource group scope) and access to Microsoft Sentinel workspace.
- Teams channel where the connector will post.
- Parameters: resource group name, location, Sentinel workspace ID, Teams team/channel IDs.

<details>
<summary>Show exact prerequisites</summary>

- Entra app registration (if applicable): `App.Read.All`, `ChannelMessage.Send`.
- Service connection: Managed Identity enabled on the Logic App.
- Optional: Create a dedicated resource group (e.g., `rg-sec-alerts-prod-uks`).

</details>

### 1) Deploy the resources
1. **Open** the template in Azure Portal:  
   `Deployments → Create → Template (ARM/Bicep)`
2. **Upload** `./samples/azuredeploy.json` (or Bicep) and **Review + create**.
3. **Fill parameters**:
   - `workspaceName`: `<your-sentinel>`
   - `logicAppName`: `la-alerts-to-teams`
   - `location`: `UK South`
4. **Create** the deployment and wait for `Succeeded`.

<details>
<summary>CLI alternative</summary>

```bash
az group create -n rg-sec-alerts-prod-uks -l uksouth
az deployment group create \
  -g rg-sec-alerts-prod-uks \
  -f ./samples/azuredeploy.json \
  -p workspaceName=<your-sentinel> logicAppName=la-alerts-to-teams location="UK South"
```
</details>

### 2) Connect to Teams
1. Go to Logic Apps → `la-alerts-to-teams` → Workflows → `notify-teams`.
2. Open the Teams connector action and **Sign in** / select the **Team** and **Channel**.
3. Save the workflow.

<details>
<summary>Find Team/Channel IDs</summary>

- In Teams: **…** next to the team → **Get link to team** (ID in the URL).  
- Or use Graph/CLI if you prefer.

</details>

### 3) Wire to Sentinel alerts
1. In **Microsoft Sentinel** → **Analytics**, create/enable alert rules to trigger the playbook.
2. For each rule, set **Automated response** → **Add playbook** → select `notify-teams`.
3. Save and enable the rule.

### 4) Test
1. Run the workflow with a sample payload (Logic App **Run Trigger**).
2. Verify a message appears in Teams.
3. If missing fields, check the **Runs history** → **Inputs/Outputs** for mapping errors.

### 5) Tuning (optional)
- **Rate-limit**: add a trigger concurrency cap (e.g., `1`) to avoid Teams payload bursts.  
- **Payload size**: cap list lengths (e.g., 10–20 items) to stay below the Teams 28 KB limit.  
- **Logging**: enable diagnostic logs → Log Analytics → `WorkflowRuntime` table.  
- **Security**: restrict Managed Identity and connector permissions to least privilege.

---

✅ **Done!** Your Logic App should now post Sentinel alerts directly into your Teams channel.
