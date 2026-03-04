# Trust Policy — MCP Servers

## Eligibility Rule

A candidate MCP server is eligible if any one of these is true:

1. Publisher is on the allowlist.
2. Weekly npm downloads >= 500.
3. GitHub stars >= 300.

If none are true, candidate is not eligible.

## Missing Metadata

- Missing downloads -> use stars and allowlist only.
- Missing stars -> use downloads and allowlist only.
- Missing downloads and stars -> only allowlist can pass.
- Missing publisher -> cannot allowlist match.

Default numeric fallback for missing downloads/stars is `0`.

## Lookup Failures

- If metadata lookup fails, mark `trust_unknown`.
- `trust_unknown` is treated as not eligible by default.
- If cached metadata exists, use it and mark source as `cache`.

## Editable Publisher Allowlist (Example)

Adjust this list to match your own risk posture.

### Tier 1: Protocol authors and stewards

- modelcontextprotocol
- anthropic

### Tier 2: Major platform and cloud vendors

- microsoft
- google
- github
- aws
- cloudflare
- vercel
- supabase
- netlify
- hashicorp

### Tier 3: Domain-leading SaaS with official MCP servers

- stripe
- shopify
- twilio
- datadog
- sentry
- linear
- notion
- slack
- atlassian
- grafana
- elastic
- mongodb
- prisma
- neon

## Package Scope Trust

npm packages under the `@modelcontextprotocol/` scope receive automatic allowlist treatment because the scope is controlled by the protocol maintainers.

## Security Caveat

This policy is a heuristic, not a guarantee of safety. Passing trust filters does not guarantee code quality, security, or compatibility. MCP servers execute code locally and may access files, databases, or external APIs. Keep normal review and validation practices. Review the server's declared tool list and permissions before confirming installation.
