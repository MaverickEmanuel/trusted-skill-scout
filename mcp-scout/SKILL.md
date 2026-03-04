---
name: trusted-mcp-scout
description: Discover, trust-filter, and configure useful MCP servers for any repository. Use this whenever a user asks to find MCP servers, compare server options, or set up MCP integrations for their current project, even if they do not mention "trust" or "security" explicitly.
allowed-tools:
  - Read
  - Glob
  - Grep
  - Bash
  - Task
  - WebFetch
---

# Trusted MCP Scout

Find high-signal MCP servers for the current repository, filter by trust policy, collect user selections, and configure deterministically.

## Core Behavior

Follow this flow in order. Do not skip gates.

1. Ask config scope before discovery (default: project-only).
2. Profile the repo (parallel sub-agents when available).
3. Generate 6-10 repo-aware discovery queries.
4. Run discovery queries sequentially across MCP registries.
5. Enrich candidates with trust metadata.
6. Apply trust filter.
7. Rank, present compact cards, and ask `Keep: ...`.
8. Build final shortlist and ask install confirmation.
9. Configure selected servers sequentially with deterministic commands.
10. Verify and report results.

## Step 0: Ask Scope First

Always ask exactly once at the beginning:

`Configure MCP servers as Project-only or Global? (default: Project-only)`

Explain the difference:
- **Project-only**: writes to `.mcp.json` in the repo root. Version-controllable, scoped to this project.
- **Global**: writes to `claude_desktop_config.json`. Available across all projects in Claude Desktop.

Rules:
- If user does not choose explicitly, use `Project-only`.
- Persist this choice for all install commands in this run.

## Step 1: Profile Repository

### Preferred mode: parallel sub-agents

If the `Task` tool is available, spawn sub-agents in parallel:

- Sub-agent A: stack and runtime detection (languages, frameworks, package managers).
- Sub-agent B: data layer and service detection (databases, APIs, cloud providers, SaaS integrations).
- Sub-agent C: workflow and operational detection (`test`, `lint`, `build`, `deploy`, CI, release).
- Sub-agent D: product intent and priorities (README, docs, issues, architecture clues).

Collect and merge into a single repo profile:

- `stack`: key technologies and frameworks
- `data_layer`: databases, caches, and external data stores
- `services`: third-party APIs and SaaS integrations detected
- `workflows`: key engineering workflows
- `goals`: likely user goals and pain points
- `constraints`: notable constraints (legacy, monorepo, compliance, hosting platform, etc.)

### Fallback mode: serial profiling

If sub-agents are unavailable, run the same profiling dimensions serially.

### Data layer detection hints

Look for these signals to identify databases and services:

- `DATABASE_URL`, `POSTGRES_*`, `MYSQL_*`, `MONGO_*` in `.env.example` or config
- `prisma/schema.prisma`, `drizzle.config.ts`, `knexfile.js`
- `docker-compose.yml` service definitions
- Import patterns: `pg`, `mysql2`, `mongoose`, `redis`, `@aws-sdk/*`
- CI/CD config referencing external services

## Step 2: Generate Discovery Queries (6-10)

Generate 6-10 specific queries based on the repo profile.

Good query dimensions:
- detected databases (postgres, mysql, mongodb, redis, sqlite)
- version control platform (github, gitlab)
- file system operations
- cloud/deployment platform (aws, vercel, cloudflare)
- monitoring and error tracking (sentry, datadog)
- communication tools (slack, discord)
- search and knowledge retrieval
- domain-specific integrations (stripe, twilio, etc.)
- memory and context persistence

Do not use a fixed one-size-fits-all query list.

## Step 3: Run Discovery Sequentially

For each query, search for MCP servers using available sources in order:

1. **npm registry**: `npm search mcp-server <query> --json` for package metadata and download counts.
2. **Web registries**: fetch `https://mcp.so`, `https://glama.ai/mcp/servers`, or `https://smithery.ai` for curated listings.
3. **GitHub search**: search `github.com/topics/mcp-server` or the `modelcontextprotocol` org for source repos.

Process queries one at a time.

From output, extract candidate rows with:
- `publisher` (npm scope or GitHub org/user)
- `package_name` (npm package name or repo name)
- `downloads` (weekly npm downloads if available)
- `stars` (GitHub stars if available)
- `description`
- `install_command` (the npx/uvx/docker command to run the server)

Normalization rules:
- Parse downloads as an integer; `K/M` suffixes should be converted to absolute values.
- If downloads missing, set downloads to `0`.
- If stars missing, set stars to `0`.

## Step 4: Enrich Candidate Metadata

For each candidate, collect:
- `publisher`
- `package_name`
- `downloads` (weekly npm downloads)
- `stars` (GitHub stars)
- `description`
- `install_transport` (stdio, http, or sse)
- `install_command`
- `required_env` (environment variables needed, e.g., API keys, tokens)
- `tools_exposed` (list of MCP tools the server provides, if discoverable)

Recommended enrichment order:
1. Check npm registry for download counts and package metadata.
2. Check GitHub repo for stars, recent activity, and README.
3. If the server has a listing on mcp.so or glama.ai, fetch additional metadata.

If downloads or stars are missing after enrichment, set to `0`.

## Step 5: Trust Filter (Hard Gate)

Use policy from `references/trust-policy.md`.

A candidate is eligible only if at least one is true:
- publisher is allowlisted
- weekly npm downloads >= 500
- GitHub stars >= 300

Packages under the `@modelcontextprotocol/` npm scope are automatically allowlisted.

Missing downloads/stars count as `0` unless allowlisted.
Candidates that fail are excluded from user options.

## Step 6: Fit Scoring and Ranking

Use rubric from `references/scoring-rubric.md`.

Score each eligible candidate across:
- Stack Fit (1-5)
- Expected Impact (1-5)
- Overlap Risk (1-5; lower is better)
- Implementation Clarity (1-5)

Compute:
- `overlap_bonus = 6 - overlap_risk`
- `fit_total = stack_fit + expected_impact + overlap_bonus + implementation_clarity` (max 20)

Verdict:
- `Strong fit` if `fit_total >= 15`
- `Maybe` if `fit_total >= 11`
- otherwise exclude

Sort with tie-breakers:
1. fit_total desc
2. stars desc
3. downloads desc
4. publisher/package asc

## Step 7: Present Per-Query Options

For each query, show only trust-eligible candidates as compact cards.

Card format:

`[1] publisher/package_name`
`Trust: <allowlisted|downloads>=500|stars>=300>`
`Fit: <fit_total>/20 | Verdict: <Strong fit|Maybe>`
`Why: <one line repo-specific rationale>`
`Auth: <none|requires ENV_VAR_NAME>`
`Agent recommendation: <Keep|Skip> + short reason`

Then ask:

`Your choice? Keep: 1,3 or Keep: none`

Selection rules:
- Accept only indices shown for that query.
- Continue to next query after selection.
- If no trust-eligible candidates exist for a query, say so and continue.

## Step 8: Final Shortlist

After all queries:
- dedupe selections by `(publisher, package_name)`
- provide overlap/conflict notes
- flag any auth requirements (API keys, tokens) the user will need to provide
- propose a best bundle (usually 3-5 complementary servers)
- ask one final gate:

`Install these now? Yes / Revise list`

Proceed only on `Yes`.

## Step 9: Configure Sequentially (Deterministic)

Configure each selected server sequentially.

### Project-only scope (`.mcp.json`)

Use `claude mcp add` for each server:

```
claude mcp add <server_name> -- <command> <args...>
```

If environment variables are required:

```
claude mcp add <server_name> --env KEY=value -- <command> <args...>
```

### Global scope (`claude_desktop_config.json`)

Use `claude mcp add --scope user` for each server:

```
claude mcp add <server_name> --scope user -- <command> <args...>
```

### Common install patterns

npx-based (most common):
```
claude mcp add postgres -- npx -y @modelcontextprotocol/server-postgres postgresql://localhost:5432/mydb
```

uvx-based (Python servers):
```
claude mcp add sqlite -- uvx mcp-server-sqlite --db-path /path/to/db.sqlite
```

Docker-based:
```
claude mcp add postgres -- docker run -i --rm mcp/postgres postgresql://localhost/mydb
```

If one install fails:
- log failure
- continue remaining installs
- provide exact retry command for failed item

## Step 10: Verify and Report

Verification commands:
- List configured servers: `claude mcp list`
- Check server connectivity: restart Claude Code session and confirm tools appear

Return final report:
- configured successfully (with server names)
- failed configurations with likely cause
- retry commands for failures
- auth setup reminders (environment variables that still need values)
- next step: `Restart Claude Code to activate new MCP servers`

## Output Template

Use concise sections:

1. Scope
- selected scope (Project-only or Global)
- config file target (`.mcp.json` or `claude_desktop_config.json`)

2. Repo Profile
- stack
- data layer
- services
- workflows
- goals

3. Query N Results
- ranked eligible cards
- one explicit agent pick
- `Your choice? Keep: ...`

4. Final Shortlist
- deduped list with auth requirements noted
- overlap notes
- recommended bundle
- install confirmation prompt

5. Configuration Report
- per-server result
- verification summary
- auth setup reminders
- restart instruction
