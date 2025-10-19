{
  "$schema": "http://adaptivecards.io/schemas/adaptive-card.json",
  "type": "AdaptiveCard",
  "version": "1.5",
  "body": [
    {
      "type": "TextBlock",
      "text": "Risky users (updates)",
      "weight": "Bolder",
      "size": "Medium"
    },
    {
      "type": "TextBlock",
      "spacing": "None",
      "isSubtle": true,
      "text": "Scanning window: 2025-10-01T00:00:00Z → 2025-10-01T12:00:00Z • Generated: 2025-10-01T12:00:05Z"
    },
    {
      "type": "Container",
      "style": "attention",
      "items": [
        {
          "type": "FactSet",
          "facts": [
            { "title": "User", "value": "user@example.org" },
            { "title": "Risk", "value": "high" },
            { "title": "State", "value": "atRisk" },
            { "title": "Detail", "value": "none" },
            { "title": "UTC", "value": "2025-10-01T11:56:24Z" }
          ]
        }
      ]
    },
    {
      "type": "TextBlock",
      "text": "Risky sign-ins (updates)",
      "weight": "Bolder",
      "size": "Medium",
      "spacing": "Large"
    },
    {
      "type": "Container",
      "style": "warning",
      "items": [
        {
          "type": "FactSet",
          "facts": [
            { "title": "User", "value": "another.user@example.org" },
            { "title": "Risk", "value": "medium" },
            { "title": "State", "value": "atRisk" },
            { "title": "Detail", "value": "none" },
            { "title": "IP", "value": "203.0.113.10" },
            { "title": "Location", "value": "[REDACTED]" },
            { "title": "UTC", "value": "2025-10-01T11:39:53Z" },
            { "title": "Local (Europe/London)", "value": "2025-10-01T12:39:53Z" }
          ]
        }
      ]
    }
  ],
  "actions": [
    {
      "type": "Action.OpenUrl",
      "title": "Open Entra (filtered)",
      "url": "https://entra.microsoft.com/#view/Microsoft_AAD_IAM/IdentityRiskMenuBlade/~/Users"
    }
  ]
}
