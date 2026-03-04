# Sample Session

## Scenario

- Repo: TypeScript API service
- User goal: improve auth and API hardening workflows

## Transcript

User: Find skills that can help this repo with auth hardening and safer API releases.

Assistant: Install approved skills as Global or Project-only? (default: Project-only)

User: default is good

Assistant: Great, using Project-only.

Assistant: Repo profile
- Stack: Node.js, TypeScript, Express
- Workflows: Jest tests, lint, GitHub Actions
- Goal: auth hardening and reliable release process

Assistant: Query 1/6: `express jwt auth security skill`

[1] owner-a/security-skills@api-auth-hardening
Trust: installs>=100
Fit: 16/20 | Verdict: Strong fit
Why: directly targets JWT middleware and route protection.
Agent recommendation: Keep - high fit, low overlap.

[2] owner-b/dev-skills@secure-http-checklist
Trust: stars>=500
Fit: 13/20 | Verdict: Maybe
Why: useful hardening checklist, but partially overlaps existing lint rules.
Agent recommendation: Skip - moderate overlap.

Your choice? Keep: 1,3 or Keep: none

User: Keep: 1

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
