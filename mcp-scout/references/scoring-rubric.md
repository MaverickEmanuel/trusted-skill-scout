# Scoring Rubric — MCP Servers

All dimensions are scored 1-5.

## Dimensions

### 1) Stack Fit

- 1: unrelated to repo stack or workflows
- 2: weak fit, requires significant setup or adaptation
- 3: partial fit, useful but not core
- 4: good fit, directly supports detected stack
- 5: excellent native fit, purpose-built for detected stack/runtime

### 2) Expected Impact

- 1: little practical value for this repo
- 2: minor quality-of-life improvement
- 3: moderate workflow improvement
- 4: strong leverage on a core workflow (db access, API integration, CI)
- 5: high leverage across multiple daily workflows

### 3) Overlap Risk

- 1: very high overlap with already-configured MCP servers or IDE features
- 2: high overlap
- 3: moderate overlap
- 4: low overlap
- 5: minimal overlap, fills a clear gap

Note: lower overlap risk is better, so scoring uses `overlap_bonus = 6 - overlap_risk`.

### 4) Implementation Clarity

- 1: unclear setup, missing docs, complex auth requirements
- 2: partial guidance, some manual configuration needed
- 3: usable but some ambiguity in config or permissions
- 4: clear setup path, well-documented config
- 5: very clear, zero-config or single-command install

## Formula

- `overlap_bonus = 6 - overlap_risk`
- `fit_total = stack_fit + expected_impact + overlap_bonus + implementation_clarity`
- Range: 4-20

## Verdict Mapping

- Strong fit: `fit_total >= 15`
- Maybe: `fit_total >= 11 and < 15`
- Exclude: `< 11`

## Tie-Breakers

When totals tie, sort by:

1. stars desc
2. downloads desc
3. publisher/package asc

## Agent Recommendation Guidance

- Keep: Strong fit with clear trust reason.
- Skip: Maybe with high overlap, complex auth setup, or weak immediate value.
