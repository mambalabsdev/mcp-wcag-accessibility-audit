# WCAG Accessibility Audit MCP Server

[![LightNow capabilities](https://lightnow.ai/badge/com.mambabuilt/mcp-wcag-accessibility-audit)](https://lightnow.ai/servers/com.mambabuilt/mcp-wcag-accessibility-audit) [![Glama score](https://glama.ai/mcp/servers/mambalabsdev/mcp-wcag-accessibility-audit/badges/score.svg)](https://glama.ai/mcp/servers/mambalabsdev/mcp-wcag-accessibility-audit) [![MCP Registry](https://img.shields.io/badge/dynamic/json?url=https%3A%2F%2Fregistry.modelcontextprotocol.io%2Fv0%2Fservers%3Fsearch%3Dcom.mambabuilt%252Fmcp-wcag-accessibility-audit%26limit%3D1&query=%24.servers%5B0%5D._meta%5B%22io.modelcontextprotocol.registry%2Fofficial%22%5D.status&label=mcp%20registry&color=blue)](https://registry.modelcontextprotocol.io/v0/servers?search=com.mambabuilt/mcp-wcag-accessibility-audit&limit=1) [![npm version](https://img.shields.io/npm/v/@mambalabsdev/mcp-wcag-accessibility-audit)](https://www.npmjs.com/package/@mambalabsdev/mcp-wcag-accessibility-audit) [![npm downloads](https://img.shields.io/npm/dm/@mambalabsdev/mcp-wcag-accessibility-audit)](https://www.npmjs.com/package/@mambalabsdev/mcp-wcag-accessibility-audit) [![license](https://img.shields.io/github/license/mambalabsdev/mcp-wcag-accessibility-audit)](https://github.com/mambalabsdev/mcp-wcag-accessibility-audit/blob/main/LICENSE) [![mcpservers.org](https://img.shields.io/badge/mcpservers.org-listed-blue)](https://mcpservers.org/servers/mambalabsdev/mcp-wcag-accessibility-audit)

<a href="https://mcprush.com/to/wcag-accessibility-audit-mcp?ref=badge"><img width="380" height="200" src="https://mcprush.com/badge/wcag-accessibility-audit-mcp/card.svg" alt="Wcag Accessibility Audit MCP server" /></a>

An MCP server for the Mamba Labs WCAG Accessibility Audit actor on Apify. Send a domain or a list. Each domain is rendered in a real browser and audited with axe-core, and you get back one row per domain: the WCAG issues automated checks find by severity, the worst failing rules, a score, European Accessibility Act exposure signals, and optional compliance page checks.

## What's Inside

- [What it does](#what-it-does)
- [Quick start](#quick-start)
- [Prerequisites](#prerequisites)
- [Example prompts](#example-prompts)
- [Tool](#tool)
- [Inputs](#inputs)
- [Pricing](#pricing)
- [Your keys](#your-keys)
- [Full actor documentation](#full-actor-documentation)
- [License](#license)

## What it does

Audit websites for accessibility with axe-core in a real browser. Give one domain or a list; each domain returns one row with the WCAG issues automated checks find (distinct failing rules by impact, affected element count, top failing rules with help links, sample selectors), a 0 to 100 score, European Accessibility Act exposure signals, and optional presence checks for an accessibility statement, an accessibility overlay widget, a privacy policy, a German Impressum, and the US privacy choice link and GPC file. Blocked, timed out, or unreachable sites return a free row that says why. Automated checks find a subset of WCAG failures and the result is never a conformance claim. A run that does not succeed is returned as an error with its run ID. Read-only on the audited sites; requires an APIFY_TOKEN and consumes Apify credits per audited domain.

All of the work runs on Apify. This package is a thin client that starts the actor run, waits for it, and hands back the dataset rows.

## Quick start

You need Node.js 18 or newer and an Apify account with an API token.

Add this to your Claude Desktop config (`claude_desktop_config.json`), Cursor, or any MCP client:

```json
{
  "mcpServers": {
    "mamba-wcag-accessibility-audit": {
      "command": "npx",
      "args": [
        "-y",
        "@mambalabsdev/mcp-wcag-accessibility-audit"
      ],
      "env": {
        "APIFY_TOKEN": "your-apify-token"
      }
    }
  }
}
```

Restart the client. The `audit_wcag_accessibility` tool appears.

## Prerequisites

- Node.js 18 or newer.
- An Apify API token from [console.apify.com/account/integrations](https://console.apify.com/account/integrations), set as `APIFY_TOKEN`. The server lists its tool without a token; a call needs one.

## Example prompts

- "Audit mambabuilt.com and example.de against WCAG 2.2 AA and list the critical issues."
- "Check these 20 EU shop domains for EAA exposure and whether each links an accessibility statement."
- "Does this German site have an Impressum, and which required fields does it show?"

## Tool

`audit_wcag_accessibility`. One tool. Example arguments:

```json
{
  "domains": ["mambabuilt.com", "example.de"],
  "standard": "wcag22aa",
  "compliance_checks": ["accessibility_statement", "impressum", "overlay"]
}
```

## Inputs

Every buyer facing input of the actor, unchanged. Omitted inputs take the actor's own defaults.

| Input | Type | Required | Description |
|---|---|---|---|
| `domain` | string | one of the two | One website domain. The homepage is audited. |
| `domains` | array | one of the two | A list of website domains, one row per domain. |
| `standard` | string | no | `wcag21aa` (default), `wcag22aa`, `wcag2aa`, `wcag2a`, `section508`, `ada`, or `en301549`. |
| `pages` | string | no | `1` to `5` pages per domain. Default `1`. |
| `viewport` | string | no | `desktop` (default) or `mobile`. |
| `compliance_checks` | array | no | Any of `accessibility_statement`, `overlay`, `privacy_policy`, `impressum`, `us_privacy_links`. |
| `skipCache` | string | no | `false` (default) reuses an identical audit from the last 24 hours; `true` audits fresh. |
| `max_charge_usd` | string | no | Optional cost cap for the run, in USD. |

## Pricing

Pay per event on Apify, billed to your Apify account. Prices in USD.

| Event | Free plan | Bronze | Silver | Gold |
|---|---|---|---|---|
| `domain-audited`, per domain with a rendered audit | $0.01 | $0.0095 | $0.009 | $0.0085 |
| `page-audited`, per page after the homepage | $0.002 | $0.0019 | $0.0018 | $0.0017 |
| `compliance-checked`, per domain with any compliance check | $0.003 | $0.00285 | $0.0027 | $0.00255 |
| `apify-actor-start`, per run start | $0.0025 | $0.0025 | $0.0025 | $0.0025 |

Blocked, timed out, and unreachable domains are never charged.

## Your keys

The actor needs no provider key. It reads only the sites you give it and holds no key of its own.

## Full actor documentation

[apify.com/mambalabs/wcag-accessibility-audit](https://apify.com/mambalabs/wcag-accessibility-audit)

## License

MIT. See [LICENSE](LICENSE).
