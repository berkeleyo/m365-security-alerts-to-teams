# FAQ

Q: Where do I get Team/Channel IDs?
A: In Teams, open the channel menu (…) → Get link to channel. The URL contains both IDs (URL-encoded).

Q: Do I need a Teams webhook?
A: No. The Logic App uses Managed Identity to call Microsoft Graph.

Q: Can I point to multiple channels?
A: Create additional playbooks or parameterize team/channel and trigger with different automation rules.

Q: How do I stop alerts temporarily?
A: Disable the automation rule (incidents will still be created unless you also disable the analytics rule).
