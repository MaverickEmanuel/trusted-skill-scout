# Query Generation Guide — MCP Servers

Generate 6-10 search queries from repo context. Avoid generic one-word searches.

## Inputs to Consider

- Framework/runtime (Next.js, Rails, Django, React Native, etc.)
- Language/tooling (TypeScript, Python, Go, package manager)
- Data stores detected (PostgreSQL, MySQL, MongoDB, Redis, SQLite)
- External services (GitHub, Slack, Jira, Linear, Stripe, AWS)
- Scripts and workflows (`test`, `lint`, `build`, `deploy`, CI)
- Domain concerns (auth, billing, data, mobile, SEO, docs)
- Current user goal in conversation

## MCP Server Categories

Map repo signals to these categories when generating queries:

- **File system**: repos with complex file operations, asset pipelines, or code generation
- **Database**: repos with detected database connections (Postgres, MySQL, MongoDB, SQLite, Redis)
- **Version control**: repos using GitHub, GitLab, or Bitbucket for PRs, issues, code review
- **Communication**: repos integrating Slack, Discord, email, or notification services
- **Cloud/Infrastructure**: repos deploying to AWS, GCP, Azure, Vercel, Cloudflare
- **Monitoring**: repos using Sentry, Datadog, Grafana, or custom logging
- **Search**: repos needing web search, documentation lookup, or knowledge retrieval
- **API integration**: repos calling third-party APIs (Stripe, Twilio, SendGrid, etc.)
- **Memory/Knowledge**: repos with complex context or multi-session workflows

## Query Patterns

- `<database> mcp server`
- `<saas_tool> mcp server`
- `<cloud_provider> mcp server`
- `mcp server <workflow> automation`
- `<framework> development mcp server`
- `<domain> mcp server tools`
- `filesystem mcp server` (common baseline for most repos)
- `github mcp server` (common baseline for repos on GitHub)

## Example Set (Template)

For a Next.js + PostgreSQL + GitHub repo:

1. `postgres mcp server`
2. `github mcp server`
3. `filesystem mcp server`
4. `vercel deployment mcp server`
5. `sentry error tracking mcp server`
6. `web search mcp server`
7. `memory context mcp server`

## Quality Rules

- Keep queries specific, not vague.
- Cover both the repo's data layer and its operational workflows.
- Avoid duplicate intent across queries.
- Always include database server queries if a database is detected.
- Always include the GitHub/GitLab server query if the repo is hosted there.
- Prefer 6-8 high-quality queries over 10 noisy ones.

## Discovery Sources (Checked in Order)

1. **npm registry**: search `npm search mcp-server <query>` — best for download counts and version info.
2. **mcp.so**: fetch `https://mcp.so` — curated directory with categories and descriptions.
3. **glama.ai**: fetch `https://glama.ai/mcp/servers` — directory with server metadata.
4. **GitHub search**: search `github.com/topics/mcp-server` or `github.com/modelcontextprotocol` — best for stars and source review.
5. **smithery.ai**: fetch `https://smithery.ai` — registry with install commands.
