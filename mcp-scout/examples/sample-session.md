# Sample Session: MCP Scout

## Scenario

- Repo: Next.js 14 app with PostgreSQL, Prisma ORM, GitHub Actions CI, Vercel deployment
- User goal: discover useful MCP servers

## Transcript

**User:** Help me find MCP servers that would be useful for this project.

**Assistant:** Configure MCP servers as Project-only or Global? (default: Project-only)

Project-only writes to `.mcp.json` in this repo. Global writes to `claude_desktop_config.json`.

**User:** Project-only is fine.

**Assistant:** Great, using Project-only scope. All servers will be added to `.mcp.json`.

**Assistant:** Profiling repository...

Repo Profile:
- Stack: Next.js 14 (App Router), TypeScript, PostgreSQL, Prisma ORM
- Workflows: Jest tests, ESLint, GitHub Actions CI/CD, Vercel deployment
- Goals: full-stack web app with relational data layer, API routes, server components
- Constraints: Vercel hosting, PostgreSQL as primary data store

**Assistant:** Generating discovery queries...

1. `postgres mcp server`
2. `github mcp server`
3. `filesystem mcp server`
4. `vercel deployment mcp server`
5. `web search mcp server`
6. `memory context mcp server`

**Assistant:** Query 1/6: `postgres mcp server`

[1] modelcontextprotocol/server-postgres
Trust: allowlisted publisher (modelcontextprotocol)
Fit: 17/20 | Verdict: Strong fit
Why: direct read-only access to the PostgreSQL database powering this app — enables schema inspection, query prototyping, and data exploration without leaving the editor.
Agent recommendation: Keep - core data layer server, high impact.

[2] neon/mcp-server-neon
Trust: allowlisted publisher (neon)
Fit: 12/20 | Verdict: Maybe
Why: Neon-specific Postgres features; useful only if migrating to Neon serverless.
Agent recommendation: Skip - not using Neon, moderate overlap with generic postgres server.

Your choice? Keep: 1 or Keep: none

**User:** Keep: 1

**Assistant:** Query 2/6: `github mcp server`

[1] github/github-mcp-server
Trust: allowlisted publisher (github)
Fit: 16/20 | Verdict: Strong fit
Why: PR creation, issue management, code search, and repo operations directly from the editor — matches the GitHub Actions CI workflow.
Agent recommendation: Keep - daily workflow server, high leverage.

Your choice? Keep: 1 or Keep: none

**User:** Keep: 1

**Assistant:** Query 3/6: `filesystem mcp server`

[1] modelcontextprotocol/server-filesystem
Trust: allowlisted publisher (modelcontextprotocol)
Fit: 15/20 | Verdict: Strong fit
Why: secure file read/write/search across the project directory with configurable access controls.
Agent recommendation: Keep - foundational server for file operations.

Your choice? Keep: 1 or Keep: none

**User:** Keep: 1

**Assistant:** Query 4/6: `vercel deployment mcp server`

No trust-eligible candidates found for this query. Continuing.

**Assistant:** Query 5/6: `web search mcp server`

[1] modelcontextprotocol/server-brave-search
Trust: allowlisted publisher (modelcontextprotocol)
Fit: 13/20 | Verdict: Maybe
Why: web and local search via Brave API — useful for docs lookup, but requires a Brave API key.
Agent recommendation: Skip - nice-to-have, requires external API key setup.

Your choice? Keep: 1 or Keep: none

**User:** Keep: none

**Assistant:** Query 6/6: `memory context mcp server`

[1] modelcontextprotocol/server-memory
Trust: allowlisted publisher (modelcontextprotocol)
Fit: 14/20 | Verdict: Maybe
Why: knowledge-graph-based persistent memory across sessions — helpful for long-running projects.
Agent recommendation: Keep - low setup cost, useful for multi-session context.

Your choice? Keep: 1 or Keep: none

**User:** Keep: 1

**Assistant:** Final shortlist:

| # | Server | Trust | Fit | Verdict |
|---|--------|-------|-----|---------|
| 1 | modelcontextprotocol/server-postgres | allowlisted | 17/20 | Strong fit |
| 2 | github/github-mcp-server | allowlisted | 16/20 | Strong fit |
| 3 | modelcontextprotocol/server-filesystem | allowlisted | 15/20 | Strong fit |
| 4 | modelcontextprotocol/server-memory | allowlisted | 14/20 | Maybe |

Overlap notes:
- No significant overlap between selected servers. Each covers a distinct capability (database, version control, file system, memory).

Bundle recommendation:
- Core: server-postgres, github-mcp-server, server-filesystem
- Optional: server-memory

Install these now? Yes / Revise list

**User:** Yes

**Assistant:** Installing to `.mcp.json`...

Adding modelcontextprotocol/server-postgres...
```
claude mcp add postgres -- npx -y @modelcontextprotocol/server-postgres postgresql://localhost:5432/myapp_dev
```
Result: added successfully.

Adding github/github-mcp-server...
```
claude mcp add github -- npx -y @modelcontextprotocol/server-github
```
Note: requires GITHUB_PERSONAL_ACCESS_TOKEN in environment.
Result: added successfully.

Adding modelcontextprotocol/server-filesystem...
```
claude mcp add filesystem -- npx -y @modelcontextprotocol/server-filesystem /home/user/myapp
```
Result: added successfully.

Adding modelcontextprotocol/server-memory...
```
claude mcp add memory -- npx -y @modelcontextprotocol/server-memory
```
Result: added successfully.

Verification:
```
claude mcp list
```

Final report:
- Installed: 4 servers
- Failed: 0
- Scope: Project-only (.mcp.json)
- Auth notes: github server requires GITHUB_PERSONAL_ACCESS_TOKEN; set via `claude mcp add github --env GITHUB_PERSONAL_ACCESS_TOKEN=ghp_... -- npx -y @modelcontextprotocol/server-github`
- Next step: restart Claude Code to activate new MCP servers
