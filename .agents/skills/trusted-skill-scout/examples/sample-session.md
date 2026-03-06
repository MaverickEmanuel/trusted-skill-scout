# Sample Session

## Scenario

- Repo: TypeScript API service
- User goal: improve auth and API hardening workflows

## Transcript

User: Find skills that can help this repo with auth hardening and safer API releases.

Assistant: Install approved skills as Global or Project-only? (default: Project-only)

User: default is good

Assistant: Great, using Project-only.

Assistant: What this skill does: discovers repo-relevant skills, applies a trust gate, and installs only the skills you explicitly approve.

Assistant: Repo profile
- Stack: Node.js, TypeScript, Express
- Workflows: Jest tests, lint, GitHub Actions
- Goal: auth hardening and reliable release process

Assistant: Query 1/6: `express jwt auth security skill`

[1] owner-a/security-skills@api-auth-hardening
Trust: installs>=100
Fit: 16/20 | Verdict: Strong fit
Why: directly targets JWT middleware and route protection.
Agent recommendation: Keep - high fit, low overlap, and highest installs.

[2] owner-b/dev-skills@secure-http-checklist
Trust: stars>=500
Fit: 16/20 | Verdict: Strong fit
Why: strong auth hardening coverage, but lower installs than option 1.
Agent recommendation: Keep - strong fit, but secondary to option 1 on popularity.

[3] owner-c/ops-skills@api-rate-limit-guardrails
Trust: installs>=100
Fit: 15/20 | Verdict: Strong fit
Why: focused on distributed rate limiting and abuse mitigation workflows.
Agent recommendation: Keep - complements auth hardening with low overlap.

[4] owner-d/platform-skills@incident-ready-api-runbooks
Trust: stars>=500
Fit: 12/20 | Verdict: Maybe
Why: useful incident patterns, but broader than immediate auth goals.
Agent recommendation: Skip - useful later, lower immediate value than 1 and 3.

Select skills to keep for this query
(Use arrow keys to move, Space to toggle, Enter to confirm)

User: (selected 1 and 3)

Assistant: Selection recorded: Keep: 1,3

... (additional query rounds)

Assistant: Final shortlist
- owner-a/security-skills@api-auth-hardening
- owner-c/release-skills@safe-release-gates

Bundle recommendation
- Core: api-auth-hardening
- Optional: safe-release-gates

Install these now? Yes / Revise list

User: Yes

Assistant: Installing...
- npx skills add owner-a/security-skills --skill "api-auth-hardening" -y
- npx skills add owner-c/release-skills --skill "safe-release-gates" -y

Verification
- npx skills list
- npx skills check

Final report
- Installed: 2
- Failed: 0
- Status: complete
