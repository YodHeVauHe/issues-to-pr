# issues2PRs

Hand your agent a GitHub issue, get a [superpowers](https://github.com/obra/superpowers)-built
pull request. One issue or a whole batch — each lands as its own PR, made the
superpowers way.

## Install

```
npx skills add https://github.com/YodHeVauHe/issues2PRs
```

Works with any agent that supports the SKILL.md format — Claude Code, ZCode,
Codex CLI, Gemini CLI, Copilot CLI, Cursor.

## Requirements

- [GitHub CLI](https://cli.github.com/) (`gh`), authenticated (`gh auth login`)
- Node.js (for `npx`, only if [superpowers](https://github.com/obra/superpowers)
  isn't installed yet — the skill installs it for your agent automatically)

## Usage

Single issue:

> Fix issue #42 in acme/widget and open a PR.

Batch — worked one at a time, each with its own branch and PR:

> Turn issues #12 and #13 in acme/widget into PRs.

## How it works

1. **Bootstraps superpowers** — detects whether the superpowers skills are
   installed in your agent's runtime; if not, installs them, and follows them
   from disk if your harness doesn't hot-load new skills.
2. **Reads the issue** with `gh`, gets into the repo (cloning if needed), and
   checks push access — branching on origin, or forking if the repo is
   read-only.
3. **Hands off to the superpowers flow, untouched**: brainstorming dialogue
   with you about the issue, a written plan you approve, TDD implementation,
   code review, branch finishing. The skill never shortens or interrupts these.
4. **Lands the PR** with `Closes #N` and reports the URL. Merging stays your
   call.

## Credit

This skill is an orchestrator for [obra/superpowers](https://github.com/obra/superpowers),
Jesse Vincent's agentic skills framework. All methodology credit belongs there.

## License

[MIT](LICENSE)
