# Contributing to issues-to-pr

Thanks for helping make issue-to-PR work better for every agent. Contributions
are welcome — pull requests are reviewed by the maintainer
([@YodHeVauHe](https://github.com/YodHeVauHe)).

## What this repo is

`issues-to-pr` is an agent skill, not an application: `SKILL.md` plus
`references/runtime-detection.md` are the product. Most contributions are
documentation changes that change agent behavior.

```
SKILL.md                        # the skill itself — the product
references/runtime-detection.md # per-runtime skill paths, detection, install
README.md                       # public face: install, usage, how it works
docs/superpowers/specs/         # design docs
docs/superpowers/plans/         # implementation plans
docs/testing/                   # subagent test evidence (baseline, hardening)
```

## Ways to contribute

- **Wording improvements** to `SKILL.md` — clearer instructions, closed loopholes
- **New runtimes** in `references/runtime-detection.md` — verified skill
  directory paths for agents we don't cover yet
- **Bug reports** — a scenario where an agent following this skill does the
  wrong thing. Include: the exact user request, the agent runtime, what the
  agent did, and what it should have done.
- **Docs** — README clarity, examples, translations of the README

## The ground rule for skill changes: test first

Skills here are **tested documentation**, following the
[superpowers writing-skills](https://github.com/obra/superpowers) method. A
change to `SKILL.md` or `references/` ships with test evidence in the PR
description:

1. **RED** — dispatch a subagent with the *current* skill text and a scenario
   that exposes the problem; quote its failing behavior verbatim.
2. **GREEN** — apply your change; dispatch the same scenario with the *new*
   text; quote the compliant behavior.

A wording change with no failing scenario behind it isn't fixing anything we
can see — the scenario is what tells us (and future readers) what the words are
for. If no scenario fails, the existing wording was already fine.

For pure docs changes (README, CONTRIBUTING), no subagent testing is needed.

## Workflow

1. Fork, then branch from `main`: `git switch -c feat/your-topic`
2. Make the change with its test evidence
3. Commit using [conventional commits](https://www.conventionalcommits.org)
   (`feat:`, `fix:`, `docs:`, `test:` — see `git log` for the house style)
4. Open a pull request against `main` describing the scenario it fixes

The maintainer reviews every PR; expect the RED/GREEN evidence to be read
closely — that's the repo's quality gate, since there is no traditional test
suite to run.

## Style

- Match the existing voice: imperative, short lines, tables for pressure/reality
  pairs
- `SKILL.md` stays lean — heavy reference material goes in `references/`
- No secrets, no telemetry, no runtime-specific assumptions without a path in
  `runtime-detection.md` to back them

## License

By contributing, you agree your contributions are licensed under the
[MIT license](LICENSE) that covers this repository.
