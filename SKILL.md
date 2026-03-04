---
name: trusted-skill-scout
description: Discover, trust-filter, and install useful skills for any repository. Use this whenever a user asks to find skills, compare skill options, or install skills safely for their current project, even if they do not mention "trust" or "security" explicitly.
allowed-tools:
  - Read
  - Glob
  - Grep
  - Bash
  - Task
  - WebFetch
---

# Trusted Skill Scout

Find high-signal skills for the current repository, filter by trust policy, collect user selections, and install deterministically.

## Core Behavior

Follow this flow in order. Do not skip gates.

1. Ask install scope before discovery (default: project-only).
2. Profile the repo (parallel sub-agents when available).
3. Generate 6-10 repo-aware discovery queries.
4. Run discovery queries sequentially with `npx skills find`.
5. Enrich candidates with trust metadata.
6. Apply trust filter.
7. Rank, present compact cards, and ask `Keep: ...`.
8. Build final shortlist and ask install confirmation.
9. Install selected skills sequentially with deterministic commands.
10. Verify and report results.

## Step 0: Ask Scope First

Always ask exactly once at the beginning:

`Install approved skills as Global or Project-only? (default: Project-only)`

Rules:
- If user does not choose explicitly, use `Project-only`.
- Persist this choice for all install commands in this run.

## Step 1: Profile Repository

### Preferred mode: parallel sub-agents

If the `Task` tool is available, spawn sub-agents in parallel:

- Sub-agent A: stack and runtime detection (languages, frameworks, package managers).
- Sub-agent B: workflow detection (`test`, `lint`, `build`, `deploy`, CI, release).
- Sub-agent C: product intent and priorities (README, docs, issues, architecture clues).

Collect and merge into a single repo profile:

- `stack`: key technologies
- `workflows`: key engineering workflows
- `goals`: likely user goals and pain points
- `constraints`: notable constraints (legacy, monorepo, compliance, etc.)

### Fallback mode: serial profiling

If sub-agents are unavailable, run the same profiling dimensions serially.

## Step 2: Generate Discovery Queries (6-10)

Generate 6-10 specific queries based on the repo profile.

Good query dimensions:
- framework best practices
- testing and QA
- deployment and CI/CD
- architecture/refactoring
- performance and observability
- domain-specific workflows (auth, payments, data pipelines, mobile release, etc.)

Do not use a fixed redesign-only query list.

## Step 3: Run Discovery Sequentially

For each query, run:

`npx skills find "<query>"`

Process queries one at a time.

From output, extract candidate rows with:
- `owner/repo@skill_name`
- installs (if present)
- skills.sh URL

Normalization rules:
- Strip ANSI color escape sequences before parsing.
- Parse installs as an integer; `K/M` suffixes should be converted to absolute values.
- If installs missing, set installs to `0`.

## Step 4: Enrich Candidate Metadata

For each candidate, collect:
- `owner`
- `repo`
- `skill_name` (the exact token after `@` from discovery output)
- `skills_url`
- `installs`
- `stars`
- `owner_type` if available

Recommended enrichment order:
1. Fetch `skills_url` and parse repository link and stars.
2. If stars still missing, query GitHub repo metadata.

If stars missing after enrichment, set stars to `0`.

## Step 5: Trust Filter (Hard Gate)

Use policy from `references/trust-policy.md`.

A candidate is eligible only if at least one is true:
- owner is allowlisted
- installs >= 100
- stars >= 500

Missing installs/stars count as `0` unless allowlisted.
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
3. installs desc
4. owner/repo asc
5. skill_name asc

## Step 7: Present Per-Query Options

For each query, show only trust-eligible candidates as compact cards.

Card format:

`[1] owner/repo@skill_name`
`Trust: <allowlist|installs>=100|stars>=500>`
`Fit: <fit_total>/20 | Verdict: <Strong fit|Maybe>`
`Why: <one line repo-specific rationale>`
`Agent recommendation: <Keep|Skip> + short reason`

Then ask:

`Your choice? Keep: 1,3 or Keep: none`

Selection rules:
- Accept only indices shown for that query.
- Continue to next query after selection.

## Step 8: Final Shortlist

After all queries:
- dedupe selections by `(owner, repo, skill_name)`
- provide overlap/conflict notes
- propose a best bundle (usually 3-5 complementary skills)
- ask one final gate:

`Install these now? Yes / Revise list`

Proceed only on `Yes`.

## Step 9: Install Sequentially (Deterministic)

Install each selected skill sequentially.

Project-only scope:

`npx skills add <owner/repo> --skill "<skill_name>" -y`

Global scope:

`npx skills add <owner/repo> --skill "<skill_name>" -g -y`

If one install fails:
- log failure
- continue remaining installs
- provide exact retry command for failed item

## Step 10: Verify and Report

Verification commands:
- Project-only: `npx skills list`
- Global: `npx skills ls -g`
- Update health: `npx skills check`

Return final report:
- installed successfully
- failed installs with likely cause
- retry commands
- optional maintenance command: `npx skills update`

## Output Template

Use concise sections:

1. Scope
- selected scope
- whether default was used

2. Repo Profile
- stack
- workflows
- goals

3. Query N Results
- ranked eligible cards
- one explicit agent pick
- `Your choice? Keep: ...`

4. Final Shortlist
- deduped list
- overlap notes
- recommended bundle
- install confirmation prompt

5. Installation Report
- per-skill result
- verification summary
- next maintenance command
