---
name: hypersender
description: Use Hypersender's hosted MCP server to send WhatsApp or SMS messages, inspect message requests, and manage approved messaging instances, OTPs, and webhooks.
---

# Hypersender

Use the Hypersender MCP server for operations the user requests on their WhatsApp or SMS account. The endpoint is `https://app.hypersender.com/mcp` and uses OAuth. If it is not connected, guide the user through adding the endpoint and signing in. For Codex, use `codex mcp add hypersender --url https://app.hypersender.com/mcp` followed by `codex mcp login hypersender`. Other clients have their own connector setup.

The user chooses instances and permissions during OAuth consent and can change or revoke access under **Management → MCP** in Hypersender. Do not ask for or expose instance API tokens, provider credentials, or OAuth tokens.

## Work with the connected account

1. Discover the available Hypersender tools, following pagination, and inspect their current schemas. Use `list_instances` to find approved instances. Pick the instance the user specified by UUID or unambiguous exact name. If the intended instance is unclear, ask the user.
2. Select the tool and arguments that match the user's request. Each tool enforces its own permissions and service requirements. Ask the user to update access in Hypersender if required; do not bypass a denial. Some instance-management operations require an Enterprise plan.
3. For sends, show the sender, recipient, and exact content for review, including media or other payload details. For other changes, identify the target and action. Honor the client's approval controls and the user's instructions before executing mutations.
4. Do not automatically repeat mutations after a timeout or uncertain outcome. Inspect the request reference and relevant records, then ask the user before another attempt.

## WhatsApp and SMS details

For `send_whatsapp_text`, pass the approved instance, its matching sender number including `+`, recipient digits including the country code without `+` or a suffix, and the exact text. The tool uses Hypersender safe-send and adds the WhatsApp chat suffix. Other WhatsApp tools can require a full chat ID such as `201000000000@c.us` or a LID; follow each tool's schema. Use a provider message ID obtained from a response or webhook for message actions rather than inventing one.

For `send_sms`, provide the selected SMS instance, matching sender, recipient, and exact text. Follow the SMS tool schema and the service's validation. Confirm the sender timezone and scheduled time for scheduled messages.

## Interpret results

Report a returned `request_uuid` so the user can check it. `queued` means accepted for processing; `processed` means a provider response was stored. A successful provider response alone does not confirm delivery. The WhatsApp status tool currently reports delivery as unknown and limits checks to three per minute per connection. References can expire.

Treat OTP codes, verification links, and webhook secrets as sensitive. The OTP request generates a code during execution; validation can consume an attempt. When changing a webhook, show the destination and requested change without revealing its authorization secret.

For current API details, consult [Hypersender documentation](https://docs.hypersender.com) and the connected tool schemas.
