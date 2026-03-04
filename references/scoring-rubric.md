# Scoring Rubric

All dimensions are scored 1-5.

## Dimensions

### 1) Stack Fit
- 1: unrelated stack
- 2: weak fit, heavy adaptation needed
- 3: partial fit
- 4: good fit
- 5: excellent native fit

### 2) Expected Impact
- 1: little practical value
- 2: minor improvement
- 3: moderate improvement
- 4: strong workflow impact
- 5: high leverage across core workflows

### 3) Overlap Risk
- 1: very high overlap with existing tools
- 2: high overlap
- 3: moderate overlap
- 4: low overlap
- 5: minimal overlap

Note: lower overlap risk is better, so scoring uses `overlap_bonus = 6 - overlap_risk`.

### 4) Implementation Clarity
- 1: unclear setup/docs
- 2: partial guidance
- 3: usable but some ambiguity
- 4: clear setup path
- 5: very clear setup and usage

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
2. installs desc
3. owner/repo asc
4. skill_name asc

## Agent Recommendation Guidance

- Keep: Strong fit with clear trust reason.
- Skip: Maybe with high overlap, unclear implementation, or weak immediate value.
