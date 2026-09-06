# DNS Doctor for Gemini CLI

A [Gemini CLI](https://geminicli.com) extension that connects the [DNS Doctor](https://dnsdoctor.dev) MCP server: 16 tools to scan, diagnose and fix a domain's email authentication (SPF, DMARC, DKIM), check a DNS change from six locations on four continents, audit an SPF include chain, and read monitoring alerts. Every verdict is deterministic and every record comes from a validating engine, never from a language model.

## Install

```bash
gemini extensions install https://github.com/dnsdoctor/gemini-cli-extension
```

No API key is needed. The extension adds the hosted MCP server over streamable HTTP, a `GEMINI.md` context file carrying the server's own usage rules, and one command:

```
/dns-doctor:scan example.com
```

## What the tools do

| Tools | What they answer |
| --- | --- |
| `scan_domain`, `get_report` | The full report: SPF, DKIM, DMARC, MX, DNS health, blacklists, domain and TLS expiry, with copy-paste fix records |
| `build_dmarc_upgrade`, `generate_dmarc_record`, `validate_dmarc_record` | The next safe DMARC rung, a record from scratch, or a check of one you have |
| `count_spf_lookups`, `audit_spf_includes` | The 10-lookup budget, and who can transitively send as the domain |
| `check_dkim_selector`, `check_record`, `check_reverse_dns`, `check_propagation` | One selector, one record, one IP, or whether a change has gone global |
| `parse_dmarc_report`, `build_parked_domain_records` | An aggregate report as a source table; the three records that stop a non-sending domain being spoofed |
| `start_monitoring_signup` | A signup link that carries the domain into paid monitoring, for a human to open |
| `get_alerts`, `get_readiness` | Monitoring reads; need an API token |

## Optional API token

An API token (Dashboard, Settings, API tokens) raises the anonymous rate limit and unlocks the two monitoring reads. Add it to your own `~/.gemini/settings.json` entry for the server; local settings override the extension's defaults:

```json
{
  "mcpServers": {
    "dns-doctor": {
      "httpUrl": "https://dnsdoctor.dev/mcp",
      "headers": { "Authorization": "Bearer dnsd_YOUR_TOKEN" }
    }
  }
}
```

## Rules the server asks the model to follow

Present any returned record verbatim. A `temperror` status is a transient lookup failure, not a failure of the domain. `not_registered: true` means the domain does not resolve. SPF is diagnose-only. A human must approve every DNS change. The full text ships in `GEMINI.md`.

Apache-2.0.
