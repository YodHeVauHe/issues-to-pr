# issues2PRs — Design

**Date:** 2026-10-25
**Status:** Approved

## Intent and why

Anyone hands their agent a GitHub issue (or several) and gets a pull request back,
built the superpowers way. The skill bootstraps superpowers if missing, fetches the
issue, then steps back and lets the normal interactive superpowers flow run —
brainstorming dialogue, plan approval, TDD, review, branch finishing — untouched.

## Goals

- Cross-runtime skill, installed via `npx skills add https://github.com/YodHeVauHe/issues2PRs`
- Single issue → single PR; or several issues → sequential loop, one branch and PR
  each — user's choice at invocation
- Works in an existing checkout or clones the issue's repo if the agent isn't in one
- Push access → branch on origin; no access → fork and PR from the fork
- PR says "Closes #N"; merging stays a human call
- Interactive where the superpowers workflow calls for it; the brainstorming dialogue
  with the human is never skipped or interrupted

## Non-goals

- Does not modify, replace, or shortcut any superpowers skill — orchestrates only
- Does not merge PRs
- Does not pick or triage issues for the user
- No fully-autonomous CI mode in v1

## Anti-goals

- Interrupting or shortcutting brainstorming, plan approval, or review gates would be
  a failure even if the PR lands
- Forking the superpowers methodology into a new one

## Constraints

- SKILL.md `name` is lowercase `issues2prs` (agentskills.io spec: letters, numbers,
  hyphens only)
- Superpowers detection must work across runtime skill directories
- `gh` CLI for all issue/PR operations
- Superpowers itself installed via `npx skills add https://github.com/obra/superpowers`
- MIT license

## Approaches

- **A. Plain skill repo (chosen):** skill at repo root, installed by
  `npx skills add` — same mechanism superpowers uses. Serves Claude Code, ZCode,
  Codex, Gemini CLI, Cursor, Copilot equally.
- **B. Claude Code plugin only:** plugin manifest + marketplace. Claude-only; rejected.
- **C. Hybrid (skill + plugin metadata):** possible later addition without moving
  anything; deferred until proven.

Chose A because the audience — agents that can install superpowers — can install this
skill the same way.

## Decided how

- Public repo `YodHeVauHe/issues2PRs`: lean `SKILL.md` +
  `references/runtime-detection.md` (per-runtime skill paths, detection, install).
- Flow encoded in SKILL.md: trigger → ensure superpowers (detect, install if missing,
  fall back to reading skill files from disk if the harness doesn't hot-load new
  skills) → `gh` auth preflight → read issue(s) → ensure repo checkout (clone if
  needed) → per issue: isolated branch → normal superpowers flow (brainstorming →
  writing-plans → TDD execution → requesting-code-review →
  finishing-a-development-branch) → push (origin or fork) → `gh pr create` with
  "Closes #N" → next issue if batch → report PR URLs, stop.
- README: one-line install, usage examples, credit to obra/superpowers.
- Pressure-tested with subagents per writing-skills (baseline without the skill,
  then with, then hardening) before publishing.

## Left to the builder

- Exact SKILL.md wording and section layout, driven by the pressure tests
- Whether the multi-issue loop details live inline or in references
