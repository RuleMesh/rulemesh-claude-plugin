# RuleMesh

Engineered compliance for every regulated system.

RuleMesh is engineered compliance infrastructure. It delivers rules traced from statutory citation through control and configuration to defensible evidence, in a form engineers, AI agents, and auditors can use. GDPR and the EU AI Act are live today, with access depending on the account's plan. Paid plans also include control mappings. The rules are served from the hosted RuleMesh server over a remote MCP connection.

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

### Other clients (Codex, Cursor, VS Code, …)

This repo is the Claude plugin. For the client-agnostic remote MCP server, setup docs for other clients (OpenAI Codex, Cursor, VS Code), and the MCP Registry listing (`com.rulemesh/compliance`), see [RuleMesh/rulemesh-mcp](https://github.com/RuleMesh/rulemesh-mcp).

## What you get

The server groups its tools around one loop: plan, pull, implement, prove.

**Discovery and scoping**
- `list_regulations` — regulations available to your account
- `list_frameworks` — control frameworks available to your account
- `lookup_definition` — legal term definitions, anchored to the regulation text
- `get_compliance_plan` — a prioritised module plan grouped by risk
- `scope_next_question`, `scope_classify` — determine applicability from the regulation's own criteria
- `scope_retrieve_source` — retrieve the statutory source behind a scoping result
- `scope_query_applicable`, `scope_update_profile` — calculate applicable requirements and, when requested, save the organisation's scope profile

**Implement**
- `pull_rules` — the rules for one module: implementation checklists, expected evidence, and control mappings when the account includes them
- `scan_compliance` — requirements and evaluation guidance for Claude to apply to the repository

**Prove**
- `submit_signals` — record an evidence signal in the authenticated RuleMesh organisation
- `submit_signals_batch` — record multiple evidence signals in one call

**Track**
- `get_progress` — factual status counts, recent evidence, and recommendations across sessions
- `get_ticket_status` — Jira-linked ticket status, human verification progress, and evidence
- `get_scan_sessions`, `start_scan`, `end_scan`, `resume_session` — session lifecycle

Four prompts package the common workflows: `implement_bundle`, `scan_and_report_bundle`, `review_bundle`, and `plan_compliance`. One resource, `regulation://{id}`, returns full regulation metadata.

Claude reads your repository locally using RuleMesh requirements. RuleMesh does not read or receive source-code contents. Evidence submissions contain the evidence description and relevant filenames. They do not certify compliance or mark a requirement verified; verification remains a human action.

## Example prompts

- "Which regulations does RuleMesh support, and which can I access on my current plan?"
- "Does the EU AI Act apply to my company? Ask me the necessary scoping questions and cite the source provisions behind the result."
- "Give me a prioritised GDPR implementation plan for my application."
- "Pull the GDPR rules for my data-export feature and give me an engineering checklist."
- "Review this repository against the pulled requirements. Submit only evidence signals and filenames; do not upload source code or mark anything verified."

## Authentication

The server uses OAuth 2.1 with PKCE. Claude registers itself, opens a browser login, and exchanges a short-lived token. No API key is stored in the plugin config. A RuleMesh account is required. The FREE tier includes GDPR. EU AI Act access and control mappings are available with a paid subscription.

## Scope

GDPR and the EU AI Act are released today, with access depending on the account's plan. More regulations are in the pipeline. The rule catalog and module set grow as regulations move through the engineering process, so treat counts as current state rather than fixed.

## About RuleMesh

RuleMesh is engineered compliance infrastructure: the rule graph between regulation as written and software as built. It defines what each obligation requires, how to execute it with framework-specific controls, and what evidence proves it was done — consumable by engineers, AI agents, and auditors. → [rulemesh.com](https://rulemesh.com)

## Links

- Product: https://rulemesh.com
- Privacy: https://rulemesh.com/privacy
- Terms: https://rulemesh.com/terms
- Support: support@rulemesh.com
