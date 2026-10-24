---
name: issues2prs
description: Use when the user wants GitHub issue(s) turned into pull request(s) — fixing, implementing, or addressing an issue and opening a PR for it, whether or not the superpowers skills are installed yet. Triggers on requests like "fix issue #N and open a PR", "turn these issues into PRs", or an issue URL plus a PR request.
---

# issues2PRs

## Overview

Turn GitHub issue(s) into PR(s) the superpowers way: make sure the superpowers
skills are present (installing them if missing), read the issues, then hand the
work to the normal interactive superpowers flow — brainstorming dialogue with
the human, written plan, TDD implementation, code review, branch finishing —
untouched, and land each issue as its own PR that says `Closes #N`. This skill
orchestrates; it never replaces, shortens, or interrupts the skills it calls.
Merging stays a human call.

## Preflight

1. **Ensure superpowers.** Follow [runtime-detection.md](references/runtime-detection.md):
   detect whether the superpowers skills are installed; if not, install them
   with `npx skills add https://github.com/obra/superpowers`; if newly installed
   skills are not loadable this session, follow them from disk exactly as
   written. Do not proceed past this step without superpowers available.
2. **Ensure GitHub access.** Run `gh auth status`. If it fails, have your human
   partner authenticate (`gh auth login`) before continuing — issue reading,
   pushing, and PR creation all need it.

## Read the issues

For each issue number or URL the user gave:

```bash
gh issue view N --repo OWNER/REPO --json number,title,body,labels,comments,url
```

If the user gave no issue, ask which one(s). One run handles one issue or a
batch — whichever the user asked for.

## Get into the repo

- Already inside the issue's repository checkout: use it.
- Anywhere else: clone it (`gh repo clone OWNER/REPO`), then work from there.
- Check push access once per repo: `gh repo view OWNER/REPO --json viewerPermission`.
  `WRITE` or `ADMIN` → branch on origin. `READ` (or push later denied) → fork with
  `gh repo fork --remote` and push the fork.

## Per issue, in order

Work issues strictly one at a time, each fully finished before the next starts.
If the user gave several and no order, propose an order and let them confirm.

For each issue:

1. **Branch.** Cut `issue-N-short-slug` from the default branch — never from
   another issue's branch, never commit on the default branch.
2. **Superpowers flow, with the issue as input.** Enter the normal flow and let
   each skill run exactly as its own SKILL.md directs:
   - **brainstorming** — a dialogue with the human partner about the issue,
     before any implementation. Not a caveat for ambiguous issues: a step,
     every time. The human approves the design.
   - **writing-plans** — the approved design becomes an implementation plan.
   - **test-driven-development** — the implementation happens under TDD:
     failing test first, minimal code, refactor. Every change, every time.
   - **requesting-code-review** — the finished work gets reviewed before the PR
     exists, not after.
   - **finishing-a-development-branch** — integration is decided by that skill's
     process with the human.
3. **Land it.** Push the branch (origin, or your fork). Open the PR:

```bash
gh pr create --title "<change summary>" --body "Closes #N

<what changed and why, per the approved design>"
```

Use `--head YOUR-USERNAME:issue-N-short-slug` when PRing from a fork.

## Report

List each issue with its PR URL. Then stop. Never merge, never rebase onto the
default branch, never delete branches — those are human calls.

## Rationalizations

| Pressure | Reality |
|---|---|
| "Just make the quick fix, don't overthink it" | The superpowers gates scale down (a smaller design doc, a shorter plan); they are never skipped. "Quick" changes how much process, not whether there is any. |
| "The issue is clear, no need to brainstorm" | Clear to you ≠ agreed with the human. Brainstorming runs; its first question may be its last. |
| "Reproduction is heavy, skip to the fix" | TDD's failing test is the reproduction. Write the smallest test that fails for the issue's reason. |
| "Tests only if the project makes it trivial" | test-driven-development requires the failing-test-first cycle. Difficulty changes the test's size, not its existence. |
| "I'll fork to be safe" | Check `viewerPermission` first; fork only without write access. Forking with access leaves PRs on the wrong remote. |
| "I'll open the PR, review can happen there" | Review comes before the PR opens — that is requesting-code-review's gate, not GitHub's. |
| "Two issues overlap, I'll do them together" | One issue, one branch, one PR — always. Overlap is a question for the human during brainstorming, not a merge of work. |
| "I'll merge it since CI is green" | Merging is a human call. Report the PR and stop. |

## Non-goals

This skill does not triage or pick issues, does not modify any superpowers
skill, and has no fully-autonomous mode: the human partner is in the loop
wherever the superpowers flow puts them.
