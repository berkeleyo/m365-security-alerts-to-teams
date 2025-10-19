# Troubleshooting

## Teams post fails (403)
- The Logic App managed identity needs Graph Application permissions:
  - ChannelMessage.Send
  - Team.ReadBasic.All (or Group.Read.All)
- Grant admin consent after assigning permissions.

## No incidents created
- Make sure SigninLogs/Entra data connector is enabled in Sentinel.
- Confirm the analytics rule is enabled and time windows cover current data.

## Automation not running
- The automation rule condition must match the analytics rule (name/ID).
- Verify the rule is enabled and has a lower order than other conflicting rules.

## Wrong team/channel
- Get link to channel in Teams and copy the IDs from the URL (teamId and channelId).
