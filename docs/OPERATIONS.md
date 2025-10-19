# Operations

## Routine checks
- Logic App → Runs history: confirm recent runs succeed.
- Sentinel → Incidents: verify incidents are created from the analytics rule.
- Sentinel → Automation rules: rule is enabled and ordered correctly.
- Connections/Authorizations: Sentinel connection shows Authorized.

## Common tasks
- Change Teams target: update parameters (teamId/channelId) and redeploy the Logic App template.
- Pause notifications: disable the automation rule (keeps incidents but stops posting).
- Tune rule noise: adjust the analytics rule query, frequency, and threshold.

## Maintenance
- Review Graph permission grants periodically.
- Rotate resource names via parameters if you create separate dev/test/prod playbooks.
