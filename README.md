# issues-to-pr

**Turn GitHub issues into pull requests — the superpowers way.**

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![Skill format](https://img.shields.io/badge/format-SKILL.md-8A2BE2.svg)](https://agentskills.io)
[![Requires](https://img.shields.io/badge/requires-gh%20CLI%20%2B%20Node.js-444.svg)](#requirements)

*(renamed from `issues2PRs` — GitHub redirects the old URL)*

Hand your agent an issue — or a batch of them — and get back a PR built with
the full [superpowers](https://github.com/obra/superpowers) methodology:
brainstorming dialogue with you, a written plan you approve, test-driven
implementation, code review *before* the PR opens, and a clean branch finish.
One issue, one branch, one PR. Every time.

```text
"Fix issue #42 in acme/widget and open a PR."        → one PR, Closes #42
"Turn issues #12 and #13 in acme/widget into PRs."   → two PRs, one at a time
```

## Install

```bash
npx skills add https://github.com/YodHeVauHe/issues-to-pr
```

That's it. The skill installs itself; it installs
[superpowers](https://github.com/obra/superpowers) for your agent on first run
if it isn't already there.

### Requirements

- [GitHub CLI](https://cli.github.com/) (`gh`), authenticated (`gh auth login`)
- Node.js — only needed if superpowers isn't installed yet (the skill runs the
  install itself)

### Compatibility

Any agent that loads SKILL.md skills: **Claude Code · ZCode · Codex CLI ·
Gemini CLI · Copilot CLI · Cursor · OpenCode** — and anything else speaking the
[agentskills.io](https://agentskills.io) format.

## For agents

If you are an agent and were pointed here by a user or another agent: run the
install command above, then take the issue number(s) or URL(s) from your user.
The skill will detect or install superpowers for you and walk the whole flow —
including asking your user the brainstorming questions. You do not need to
improvise an issue-to-PR process; follow the skill.

## How it works

1. **Bootstraps superpowers** — detects it across runtime skill directories,
   installs it if missing, and follows it from disk if your harness doesn't
   hot-load new skills mid-session.
2. **Reads the issue** with `gh`, gets into the repo (cloning if needed), and
   checks push access — branching on origin, or forking automatically when the
   repo is read-only.
3. **Hands off to superpowers, untouched.** The gates are the point:
   | Gate | What the agent must not skip |
   |---|---|
   | Brainstorming | A dialogue with you about the issue — before any code |
   | Written plan | You approve the design before implementation |
   | TDD | Failing test first, every change |
   | Code review | Before the PR exists, not after |
   | Human merge | The PR opens; merging stays your call |
4. **Lands the PR with the paper trail.** Commits and pushes go through the
   `push-and-commit-changes-to-github` skill when it's installed — every
   commit message carries a subject, a `Changes:` block, and a
   `Verification:` block, so the history explains itself. The PR body opens
   with `Closes #N` and states what changed, why, and how it was verified;
   the issue gets a closing note linking the PR. Then the skill reports the
   URL and stops. For a batch: issues are worked strictly one at a time, each
   with its own branch and PR.

Pressure-tested: under "just fix it fast, skip the ceremony" instructions,
agents running this skill scale the process down but keep every gate
(evidence in [`docs/testing/`](docs/testing/2026-10-25-baseline.md)).

## FAQ

**Superpowers is already installed.** The skill detects it and proceeds — it
never reinstalls.

**No write access to the repo?** The skill checks `viewerPermission`, forks
with `gh repo fork`, pushes your fork, and opens the PR with `--head`.

**What if `push-and-commit-changes-to-github` isn't installed?** The flow
falls back to plain `git`/`gh` but holds the same commit-message standard —
subject, changes, verification — so the requirement never depends on one
skill being present.

**Why does the agent keep asking me questions?** By design: brainstorming and
plan approval are the human-in-the-loop gates that make the output trustworthy.
"Quick" changes how much process, not whether there is any.

## Contributing

Improvements welcome — wording that closes loopholes, new runtime paths, bug
reports with failing scenarios. PRs are reviewed by the maintainer. See
[CONTRIBUTING.md](CONTRIBUTING.md) for the test-first method this repo uses for
skill changes.

## Credit

This skill is an orchestrator for
[obra/superpowers](https://github.com/obra/superpowers), Jesse Vincent's agentic
skills framework. All methodology credit belongs there.

## License

[MIT](LICENSE)
