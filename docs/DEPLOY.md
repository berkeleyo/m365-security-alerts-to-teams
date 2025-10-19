# Deploy

## Prereqs
- Azure subscription + Microsoft Sentinel workspace (Log Analytics)
- Teams teamId and channelId (from “Get link to channel” in Teams)
- Azure CLI logged in (`az login`)

# What is workspaceName?
# It’s your Log Analytics workspace name (not the resource group).
# Azure Portal -> Log Analytics workspaces -> your workspace -> Overview -> Name.

-------------------------------------------------------------------------------

## Quick steps

1) Create a resource group
az group create -n rg-sec-alerts -l westeurope

2) Set Teams IDs
# Open this file locally and fill in your Teams IDs:
#   playbooks/notify-teams/parameters.example.json
# Set:
#   "teamId":     "<YOUR-TEAM-ID>"
#   "channelId":  "<YOUR-CHANNEL-ID>"

3) Deploy the Logic App (Managed Identity)
az deployment group create -g rg-sec-alerts \
  --template-file playbooks/notify-teams/azuredeploy.json \
  --parameters @playbooks/notify-teams/parameters.example.json

4) Grant Microsoft Graph permissions to the Logic App’s Managed Identity
# Portal steps (Entra ID):
# - Entra ID -> Enterprise applications -> find your Logic App (e.g., pbk-notify-teams)
# - Add Application permissions:
#     ChannelMessage.Send
#     Team.ReadBasic.All   (or Group.Read.All)
# - Click “Grant admin consent”

5) (Optional) Deploy the Sentinel analytics rule (risky sign-ins / risky users)
az deployment group create -g rg-sec-alerts \
  --template-file playbooks/notify-teams/analytics-rule-risky-signins.json \
  --parameters workspaceName=<YOUR-LAW-NAME>

# Replace <YOUR-LAW-NAME> with your Log Analytics workspace name (e.g., law-sec-prod)

6) Link the playbook with an Automation rule (in the Sentinel portal)
# Portal steps (Microsoft Sentinel):
# - Microsoft Sentinel -> your workspace -> Automation -> Create
# - When: Alert created
# - (Optional) Condition: Provider name equals "Azure Active Directory Identity Protection"
# - Action: Run playbook -> select notify-teams
# - Save

7) Validate
# - Trigger a test “risky sign-in” / Identity Protection alert (or wait for a real one)
# - Check Logic App “Runs history”
# - Confirm the Teams message arrives in the target channel

-------------------------------------------------------------------------------

## Tips

# Preview a deployment (“what-if”)
az deployment group what-if -g rg-sec-alerts \
  --template-file playbooks/notify-teams/azuredeploy.json \
  --parameters @playbooks/notify-teams/parameters.example.json

# Where to find Team/Channel IDs
# - In Teams, open the channel, click “…” -> Get link to channel
# - The URL contains both the teamId and channelId

# If you are to get a 403 from Graph:
# - Re-check the Application permissions above
# - Ensure admin consent is granted on the Enterprise app entry for your Logic App
