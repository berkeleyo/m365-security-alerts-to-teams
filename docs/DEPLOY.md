# Deploy

## Prereqs
- Azure subscription + Sentinel workspace (Log Analytics)
- Teams **teamId** and **channelId** (from *Get link to channel*)
- Azure CLI logged in (`az login`)

## Quick steps

1. Create RG:
   ```bash
   az group create -n rg-sec-alerts -l westeurope
   ```

2. Edit `playbooks/notify-teams/parameters.example.json` with your **teamId** and **channelId**.

3. Deploy the Logic App (Managed Identity):
   ```bash
   az deployment group create -g rg-sec-alerts \
     --template-file playbooks/notify-teams/azuredeploy.json \
     --parameters @playbooks/notify-teams/parameters.example.json
   ```

4. Grant Microsoft Graph permissions to the Logic App’s **Managed Identity**:
   - In Entra ID → **Enterprise applications**, find the Logic App (e.g., `pbk-notify-teams`)
   - Add **Application permissions**:
     - `ChannelMessage.Send`
     - `Team.ReadBasic.All` (or `Group.Read.All`)
   - Click **Grant admin consent**

5. (Optional) Deploy a Sentinel analytics rule:
   ```bash
   az deployment group create -g rg-sec-alerts \
     --template-file playbooks/notify-teams/analytics-rule-risky-signins.json \
     --parameters workspaceName=<YOUR-LAW-NAME>
   ```

6. Link the playbook via an **automation rule** in Sentinel:
   - Go to Sentinel → Automation → Create rule.
   - Set “When incident created from analytics rule = *Risky Sign-ins*”
   - Action: **Run playbook → notify-teams**

7. Validate:
   - Trigger a risky sign-in test event.
   - Confirm Teams notification appears with risk details.
