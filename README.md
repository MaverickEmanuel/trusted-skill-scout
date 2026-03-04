# trusted-skill-scout

Find the right skills for any repository, filter out low-trust options, and install your picks safely.

`trusted-skill-scout` is a reusable Agent Skill for people who want better signal than "just run skills find and hope." It profiles the repo, ranks relevant skills, applies a trust gate, keeps you in the loop, and installs only what you approve.

## Quickstart

```bash
npx skills add <your-username>/trusted-skill-scout -y
```

After install, the skill asks install scope inside the flow:
- `Project-only` (default, recommended)
- `Global`

## Why Use This

- Repo-aware recommendations, not random search spam
- Trust filtering before options are shown
- Human approval at every important decision point
- Deterministic install commands
- Verification output you can audit

## What It Does

1. Profiles your current repo (stack, workflow, goals)
2. Generates tailored `npx skills find` queries
3. Fetches trust metadata (installs/stars/owner)
4. Filters to trust-eligible candidates only
5. Ranks and presents compact keep/skip options
6. Installs selected skills with one final confirmation gate
7. Verifies installation and prints retry commands if needed

## Trust Model

A candidate is eligible only if at least one condition is true:

- Owner is in allowlist (major trusted orgs)
- Installs >= 100
- GitHub stars >= 500

If metadata is missing, the skill treats missing values as `0` unless owner is allowlisted.

See:
- `references/trust-policy.md`
- `references/scoring-rubric.md`

## Output Style

Per query, you get short ranked cards like:

```text
[1] owner/repo@skill-name
Trust: stars>=500
Fit: 17/20 | Verdict: Strong fit
Why: aligns with your Next.js testing + CI workflow
Agent recommendation: Keep - high fit and low overlap

Your choice? Keep: 1,3 or Keep: none
```

No noisy dumps, no auto-install surprises.

## Compatibility

- Claude Code
- OpenCode
- Other Skills-compatible agents (via Skills CLI)

## Example Session

See `examples/sample-session.md` for a full interaction transcript.

## Roadmap

- Team-level trust policy profiles
- Optional lockfile/export for reproducible bundles
- Better confidence scoring for niche domains
- Faster metadata caching for large discovery runs

## Contributing

Contributions are welcome. Start with `CONTRIBUTING.md`.

## License

MIT

---

If this skill saves you time or prevents bad installs, please star the repo.
