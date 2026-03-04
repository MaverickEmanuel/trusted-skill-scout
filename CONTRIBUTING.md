# Contributing

Thanks for helping improve `trusted-skill-scout`.

## How to Contribute

1. Fork the repo and create a branch.
2. Make focused changes.
3. Update docs/examples if behavior changes.
4. Open a PR with:
   - what changed
   - why it changed
   - how you validated it

## Contribution Areas

- Better query generation heuristics
- Better trust metadata parsing and caching
- Stronger scoring logic
- Cross-agent compatibility improvements
- Better sample sessions and eval prompts

## Quality Expectations

- Keep instructions deterministic.
- Keep user safety defaults intact.
- Do not remove human confirmation gates.
- Keep docs concise and practical.

## Local Testing Ideas

- Run `npx skills find "<query>"` and verify parser assumptions.
- Validate trust gate behavior with missing metadata cases.
- Verify install command formatting for skill names with spaces.

## Code of Conduct

Be respectful and constructive. Focus on improving reliability and user trust.
