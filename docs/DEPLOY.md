# Deploy

## Prereqs
- Azure subscription + Sentinel workspace (Log Analytics)
- Teams **teamId** and **channelId** (from *Get link to channel*)
- Azure CLI logged in (`az login`)

## Quick steps
1. Create RG:
   az group create -n rg-sec-alerts -l westeurope

2. Edit playbooks/mi-teams-alerts/parameters.example.json with your teamId and channelId.

3. Deploy the Logic App (Managed Identity):
   az deployment group create -g rg-sec-alerts \
     --template-file playbooks/mi-teams-alerts/azuredeploy.json \
     --parameters @playbooks/mi-teams-alerts/parameters.example.json

4. Grant Microsoft Graph permissions to the Logic App’s Managed Identity:
   - Entra ID → Enterprise applications → find the Logic App (e.g., pbk-mi-teams-alerts)
   - Add Application permissions:
       - ChannelMessage.Send
       - Team.ReadBasic.All (or Group.Read.All)
   - Click Grant admin consent.

5. Deploy the Sentinel analytics rule:
   az deployment group create -g rg-sec-alerts \
     --template-file playbooks/mi-teams-alerts/analytics-rule-risky-signins.json \
     --parameters workspaceName=<YOUR-LAW-NAME>

6. Link the playbook via an automation rule:
   logicId=$(az resource show -g rg-sec-alerts -n pbk-mi-teams-alerts --resource-type "Microsoft.Logic/workflows" --query id -o tsv)
   az deployment group create -g rg-sec-alerts \
     --template-file playbooks/mi-teams-alerts/automation-rule.json \
     --parameters workspaceName=<YOUR-LAW-NAME> logicAppResourceId="$logicId"

## Notes
- Get teamId/channelId from Teams → channel menu (…) → Get link to channel (the URL contains both IDs).
- No secrets or webhooks are stored; all calls use the Logic App’s Managed Identity.
- If posting fails with 403, double-check the Graph Application permissions and admin consent.
