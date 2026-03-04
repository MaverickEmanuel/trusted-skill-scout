# Trust Policy

## Eligibility Rule

A candidate is eligible if any one of these is true:

1. Owner is on the allowlist.
2. Installs >= 100.
3. GitHub stars >= 500.

If none are true, candidate is not eligible.

## Missing Metadata

- Missing installs -> use stars and allowlist only.
- Missing stars -> use installs and allowlist only.
- Missing installs and stars -> only allowlist can pass.
- Missing owner -> cannot allowlist match.

Default numeric fallback for missing installs/stars is `0`.

## Lookup Failures

- If metadata lookup fails, mark `trust_unknown`.
- `trust_unknown` is treated as not eligible by default.
- If cached metadata exists, use it and mark source as `cache`.

## Editable Owner Allowlist (Example)

Adjust this list to match your own risk posture.

- anthropic
- openai
- google
- microsoft
- github
- vercel
- cloudflare
- netlify
- shopify
- supabase
- expo

## Security Caveat

This policy is a heuristic, not a guarantee of safety. Passing trust filters does not guarantee code quality, security, or compatibility. Keep normal review and validation practices.
