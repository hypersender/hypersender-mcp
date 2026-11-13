# Hypersender MCP

Send WhatsApp and SMS messages from your AI assistant using your Hypersender account.

Hypersender MCP is a hosted [Model Context Protocol](https://modelcontextprotocol.io/) server. It connects an assistant to the WhatsApp and SMS instances you approve, with permissions you can manage from your Hypersender dashboard.

- **MCP endpoint:** `https://app.hypersender.com/mcp`
- **Transport:** Streamable HTTP
- **Authentication:** OAuth
- **Website:** [hypersender.com](https://hypersender.com)
- **Documentation:** [docs.hypersender.com](https://docs.hypersender.com)

## What you can do

- Send WhatsApp texts and other supported message types, and inspect queued request status.
- Send and manage SMS messages and conversations.
- Work with supported WhatsApp contacts, messages, and instances.
- Request and validate OTPs, and manage webhook configurations.
- Choose which instances and permissions each assistant can use, and revoke access from your dashboard.

Features depend on your subscription, the connected instance, and the permissions you grant. Some instance operations require an Enterprise plan.

## Before you start

You need a [Hypersender account](https://app.hypersender.com), a valid subscription for your chosen service, and a connected WhatsApp or SMS instance. Add the MCP endpoint to an OAuth-capable assistant, sign in, and select the instances and permissions you want to share.

Your instance API credentials remain in Hypersender. The assistant receives its own OAuth access to the instances and permissions you approve.

## Connect with Codex

Run these commands in your terminal:

```bash
codex mcp add hypersender --url https://app.hypersender.com/mcp
codex mcp login hypersender
```

Sign in to Hypersender in the browser, choose your instances and permissions, and authorize the connection. Return to Codex and start a new session.

Try asking:

> Using Hypersender, list my approved WhatsApp instances. Do not send anything.

To send a test message, replace the placeholders with an approved instance and a number you control:

> Using Hypersender and my [instance name], send “Hello from Hypersender” to [recipient number]. Show the sender, recipient, and exact message before sending. Send once.

The assistant can identify an approved instance by UUID or an unambiguous name. For WhatsApp text, use recipient digits with the country code, without a leading `+` or chat suffix.

## Connect another assistant

In an MCP-compatible client that supports remote servers and OAuth, add `https://app.hypersender.com/mcp` and follow its sign-in instructions. For Claude, add a custom connector in its connector settings and authorize your Hypersender account. Setup and approval controls vary by client.

## Permissions and approvals

Choose the operations the assistant may perform during authorization. Review or change an existing connection, or revoke it, under **Management → MCP** in the Hypersender app.

Before sending, review the sender, recipient, and message. Your assistant's tool approval settings determine how it asks for confirmation. For Codex, you can optionally configure write approvals in the existing `[mcp_servers.hypersender]` section of `~/.codex/config.toml`:

```toml
default_tools_approval_mode = "writes"
```

## Request status

WhatsApp text sends return a request reference and a queued status. Ask your assistant to check that reference with `get_whatsapp_request_status`. A processed request means the provider responded. Delivery requires a delivery acknowledgement. Request references can expire.

Avoid repeating a send automatically when its outcome is uncertain. Check the request status and recipient before deciding whether to send again.

## Support

Visit the [Hypersender documentation](https://docs.hypersender.com) or contact [support@hypersender.com](mailto:support@hypersender.com).

This repository documents the hosted Hypersender MCP integration. The server is maintained as part of the Hypersender application.
