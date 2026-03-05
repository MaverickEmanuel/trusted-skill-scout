<div align="center">

# trusted-skill-scout

Find trusted, high-signal agent skills for any repo, then install only what you approve.

[![GitHub stars](https://img.shields.io/github/stars/MaverickEmanuel/trusted-skill-scout?style=flat-square)](https://github.com/MaverickEmanuel/trusted-skill-scout)

[Open on skills.sh](https://skills.sh/maverickemanuel/trusted-skill-scout/trusted-skill-scout) • [Star on GitHub](https://github.com/MaverickEmanuel/trusted-skill-scout) • [Read the spec](./SKILL.md)

</div>

> [!TIP]
> **Quick install (project scope, recommended)**
> ```bash
> npx skills add MaverickEmanuel/trusted-skill-scout
> ```

<details>
<summary><strong>Other install options</strong></summary>

Global install:

```bash
npx skills add MaverickEmanuel/trusted-skill-scout -g
```

Install a specific skill from this repo (when applicable):

```bash
npx skills add MaverickEmanuel/trusted-skill-scout --skill "<skill_name>"
```

</details>

## Why this skill

- Repo-aware discovery (stack + workflows + goals), not generic query spam.
- Hard trust gate before options are shown.
- Human confirmation at key steps (`Keep` picks + final install gate).
- Deterministic install and verification commands with retry guidance on failure.

## How it works

1. Ask install scope first (`Project-only` default).
2. State purpose clearly: discover repo-relevant skills, trust-filter them, and install only what you approve.
3. Profile the repo and generate 6-10 focused discovery queries.
4. Trust-filter and rank candidates with transparent fit scoring; if fit ties, prioritize popularity (`installs`, then `stars`).
5. Show up to 4 trust-eligible options per query (target 3-4 when available) and collect choices with interactive multi-select when supported; fall back to deterministic text input (`Keep: 1,3` or `Keep: none`) when interactive prompts are unavailable.
6. Install only approved skills, then verify with `npx skills list` and `npx skills check`.

## Trust model

A candidate is eligible if **any** of these is true:

- owner is allowlisted
- installs `>= 100`
- GitHub stars `>= 500`

Missing numeric metadata is treated as `0` unless allowlisted.

> [!IMPORTANT]
> Trust filtering is a heuristic, not a security guarantee. Keep normal review and validation practices.

## Example interaction

```text
[1] owner/repo@skill_name
Trust: stars>=500
Fit: 17/20 | Verdict: Strong fit
Why: matches this repo's test + CI workflow
Agent recommendation: Keep - high impact, low overlap

Select skills to keep for this query
(Use arrow keys to move, Space to toggle, Enter to confirm)

Fallback: Your choice? Keep: 1,3 or Keep: none
```

## Learn more

- Behavior contract: `SKILL.md`
- Trust policy: `references/trust-policy.md`
- Scoring rubric: `references/scoring-rubric.md`
- Query generation: `references/query-generation.md`
- Full transcript: `examples/sample-session.md`

If this skill saves you time, please star the repo.
