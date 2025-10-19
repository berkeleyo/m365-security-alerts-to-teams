# Security

## Identity model
- Uses Azure **Managed Identity** for runtime access (no static secrets or incoming webhooks).

## Least privilege
- Grant only the minimal permissions required to post to Teams and read the necessary risk signals.
- Scope any permissions at the narrowest level possible.

## Data handling
- Do **not** commit real user data, IPs, locations, or timestamps.
- Use redacted examples for docs (see `EXAMPLE_CARD.md` / `EXAMPLE_CARD.json`).

## Network
- No inbound exposure; outbound access is required only for Microsoft services used by the workflow.
