# RuleMesh Compliance

Engineered Compliance Infrastructure in your editor.

RuleMesh delivers engineered rules for GDPR: what each obligation requires, how to execute it with framework-specific controls, and what evidence proves it was done. The rules are consumable by engineers and AI agents, served from the hosted RuleMesh server over a remote MCP connection.

This plugin connects Claude (Code or Desktop) to that server. You authenticate once in the browser, then your agent can pull rules, implement against them, and submit evidence as it works.

## Install

### Claude Code

Add the marketplace, then install the plugin:

```bash
/plugin marketplace add rulemesh/rulemesh-claude-plugin
/plugin install rulemesh-compliance@rulemesh
```

Or add the server directly without the plugin wrapper:

```bash
claude mcp add --transport http rulemesh https://api.rulemesh.com/mcp
```

Run `/mcp` and follow the browser login to authenticate.

### Claude Desktop

Add to `claude_desktop_config.json`:

```json
{
  "mcpServers": {
    "rulemesh": {
      "type": "streamable-http",
      "url": "https://api.rulemesh.com/mcp"
    }
  }
}
```

### OpenAI Codex

Codex supports remote MCP servers with OAuth. Add to `~/.codex/config.toml`:

```toml
[features]
rmcp_client = true            # enables the remote MCP client (use experimental_use_rmcp_client = true on older Codex)

[mcp_servers.rulemesh]
url = "https://api.rulemesh.com/mcp"
startup_timeout_sec = 30
tool_timeout_sec = 120
```

Then authenticate (opens the browser for OAuth — email/password or Google):

```bash
codex mcp login rulemesh
```

Run `/mcp` in the Codex TUI to confirm the RuleMesh tools are loaded.

### Other MCP clients

Any client that speaks Streamable HTTP can connect to `https://api.rulemesh.com/mcp`. RuleMesh is published to the [MCP Registry](https://registry.modelcontextprotocol.io) as `com.rulemesh/compliance`.

## What you get

The server groups its tools around one loop: plan, pull, implement, prove.

**Discovery**
- `list_regulations` — regulations available to your account
- `list_frameworks` — security frameworks mapped into the rules (AWS, Azure, NIST CSF, OWASP)
- `lookup_definition` — legal term definitions, anchored to the regulation text
- `get_compliance_plan` — a prioritised bundle plan grouped by risk

**Implement**
- `pull_rules` — the rules for one bundle: checklists, control mappings, and an evidence template
- `scan_compliance` — raw requirements for the agent to evaluate code against

**Prove**
- `submit_signals` — record evidence that a control was implemented
- `submit_signals_batch` — submit up to 100 evidence items in one call

**Track**
- `get_progress` — score, status breakdown, and high-risk items across sessions
- `get_ticket_status` — Jira ticket and checklist verification per bundle
- `get_scan_sessions`, `start_scan`, `end_scan`, `resume_session` — session lifecycle

Four prompts package the common workflows: `implement_bundle`, `scan_and_report_bundle`, `review_bundle`, and `plan_compliance`. One resource, `regulation://{id}`, returns full regulation metadata.

## Authentication

The server uses OAuth 2.1 with PKCE. Claude registers itself, opens a browser login, and exchanges a short-lived token. No API key is stored in the plugin config. A RuleMesh account is required; the FREE tier covers GDPR and all mapped frameworks.

## Scope

GDPR is packaged end to end today. More regulations are in the pipeline. The rule catalog and bundle set grow as regulations move through the engineering process, so treat counts as current state rather than fixed.

## Links

- Product: https://rulemesh.com
- Privacy: https://rulemesh.com/privacy
- Terms: https://rulemesh.com/terms
- Support: support@rulemesh.com