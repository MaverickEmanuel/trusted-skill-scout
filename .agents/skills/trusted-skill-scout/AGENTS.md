# AGENTS.md

Agent playbook for working in `trusted-skill-scout`.
This repository is a skill specification and reference repo, not an app runtime.
## 1) Repository Purpose
- Primary artifact: `SKILL.md` (behavior contract for the skill).
- Supporting artifacts: `references/*.md`, `examples/sample-session.md`, `evals/evals.json`.
- Main objective: keep trust-filtered skill discovery deterministic, auditable, and human-gated.
## 2) Read-First Files (In Order)
1. `SKILL.md`
2. `references/trust-policy.md`
3. `references/scoring-rubric.md`
4. `references/query-generation.md`
5. `README.md`
6. `CONTRIBUTING.md`
7. `evals/evals.json`
When changing behavior, update all impacted docs and examples in the same PR.
## 3) Build, Lint, and Test Commands
This repo currently has no dedicated build system or linter configuration.
### Build
- No `build` script is defined in this repository.
- If you add generated artifacts in the future, document generation steps in this file and `README.md`.
### Lint
- No formal lint command is configured (`eslint`, `ruff`, `markdownlint`, etc. are not set up here).
- Treat deterministic wording, valid Markdown, and valid JSON as the baseline quality bar.
### Test
- No automated test runner is configured (no `jest`, `vitest`, `pytest`, etc.).
- Testing is scenario-driven via `evals/evals.json` plus command-level validation.
### Operational Commands Used in This Repo
```bash
# Discover candidate skills for a focused query
npx skills find "<query>"

# Install this skill in a project
npx skills add <your-username>/trusted-skill-scout -y

# Install selected skill in project scope
npx skills add <owner/repo> --skill "<skill_name>" -y

# Install selected skill in global scope
npx skills add <owner/repo> --skill "<skill_name>" -g -y

# Verify installed skills (project scope)
npx skills list

# Verify installed skills (global scope)
npx skills ls -g

# Check health of installed skills
npx skills check

# Optional maintenance
npx skills update
```
## 4) Running a "Single Test" in This Repo
There is no one-command single-test framework here.
Use one eval scenario from `evals/evals.json` as the smallest test unit.
Recommended single-test workflow:
1. Pick one eval object (for example `id: 2`) in `evals/evals.json`.
2. Use the eval prompt as the user request in an agent session.
3. Validate output against that eval's `expectations` list.
4. Run command-level sanity checks as needed:
   - `npx skills find "<query from the session>"`
   - `npx skills list`
   - `npx skills check`
Pass criteria for a single eval case:
- Scope question appears first (default project-only).
- Trust filter is applied before options are shown.
- Keep/skip interaction is explicit.
- Install commands are deterministic and correctly scoped.
## 5) Code Style and Authoring Guidelines
Because this repo is mostly Markdown + JSON, style consistency matters more than framework idioms.
### Formatting
- Use Markdown headings in logical order (`#`, `##`, `###`), no skipped levels.
- Keep bullets concise and action-oriented.
- Use backticks for commands, paths, and literal tokens.
- Prefer fenced code blocks with language labels (`bash`, `json`, `text`).
- Keep line wrapping readable; avoid extremely long lines unless command literals require it.
### Imports and Module Layout (If Code Is Added Later)
- Group imports as: standard library, third-party, local modules.
- Keep imports explicit and minimal; remove dead imports.
- Prefer named exports over default exports for shared utilities.
- Keep side-effect imports rare and clearly justified.
### Types and Data Shapes
- Preserve stable field names already used in docs and evals:
  - `skill_name`, `owner`, `repo`, `installs`, `stars`, `fit_total`.
- Prefer explicit typed structures over loosely shaped objects if adding TS/JS.
- Use integers for install/star counts after normalization.
- Treat missing numeric metadata as `0` unless allowlist overrides trust status.
### Naming Conventions
- Keep JSON keys and rubric terms in `snake_case` when already defined that way.
- Keep command placeholders in angle brackets: `<owner/repo>`, `<skill_name>`, `<query>`.
- Use clear, deterministic labels in user-facing cards:
  - `Trust:`
  - `Fit:`
  - `Verdict:`
  - `Agent recommendation:`
### Error Handling and Reliability
- Never skip human confirmation gates described in `SKILL.md`.
- If metadata lookup fails, treat candidate as `trust_unknown` and not eligible by default.
- On install failures, continue remaining installs and provide exact retry commands.
- Prefer deterministic fallbacks over silent behavior changes.
- Document edge-case handling in the same change where logic is introduced.
### Determinism Rules
- Keep trust gate logic aligned with `references/trust-policy.md`.
- Keep scoring formula aligned with `references/scoring-rubric.md`.
- Keep query generation aligned with `references/query-generation.md`.
- Do not introduce random ranking behavior.
## 6) Sync Requirements for Changes
If you modify one of these, update the others when applicable:
- `SKILL.md` behavior steps
- `references/*.md` policy/rubric details
- `README.md` user-facing behavior claims
- `examples/sample-session.md` transcript realism
- `evals/evals.json` expectations
## 7) Safety and Review Checklist
Before finishing a change:
1. Confirm instructions remain deterministic.
2. Confirm trust hard gate remains enforced.
3. Confirm human-in-the-loop prompts still exist.
4. Confirm command examples are syntactically correct.
5. Confirm JSON files remain valid JSON.
6. Confirm no contradictory wording across docs.
## 8) Cursor and Copilot Rules
- No `.cursorrules` file found.
- No `.cursor/rules/` directory found.
- No `.github/copilot-instructions.md` file found.
If these are added later, treat them as authoritative constraints and update this file.
## 9) Non-Goals for Agents
- Do not invent unsupported CLI commands and present them as existing.
- Do not weaken trust policy for convenience.
- Do not auto-install without explicit final confirmation.
- Do not present untrusted candidates as eligible options.
## 10) Preferred Commit Scope
- Keep edits focused and small.
- Couple behavior changes with docs/evals updates.
- Include a short validation note in PR description (what was checked and how).

## 11) Lightweight Validation Commands

Use these for quick local sanity checks when changing docs or evals:

```bash
# Validate JSON structure
python -m json.tool evals/evals.json >/dev/null

# Spot-check discovery command behavior
npx skills find "express api auth security skill"

# Verify installed skills and health after installs
npx skills list
npx skills check
```
