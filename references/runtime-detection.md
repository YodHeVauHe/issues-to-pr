# Runtime detection: finding and installing superpowers

Superpowers (obra/superpowers) is a set of skills — brainstorming, writing-plans,
test-driven-development, requesting-code-review, finishing-a-development-branch,
and more — that this skill hands off to. It is present if a `using-superpowers`
skill exists in any of your runtime's skill directories.

## Skill directories by runtime

| Runtime          | Skill directory        | Notes                                        |
|------------------|------------------------|----------------------------------------------|
| Cross-runtime    | `~/.agents/skills/`    | Alias recognized by Codex, Copilot CLI, Gemini CLI; takes precedence when the runtime dir also exists |
| Claude Code      | `~/.claude/skills/`    |                                              |
| ZCode            | `~/.zcode/skills/`     |                                              |
| Gemini CLI       | `~/.gemini/skills/`    | Also reads `~/.agents/skills/`               |
| Codex CLI        | `~/.codex/skills/`     | Also reads `~/.agents/skills/`               |

You know your own runtime: check its directory first, then the others. A skill
directory contains one subdirectory per skill, each with a `SKILL.md`.

## Detection

Superpowers is installed if `using-superpowers/SKILL.md` exists under any of the
directories above. Check from a shell:

```bash
for d in ~/.claude/skills ~/.zcode/skills ~/.gemini/skills ~/.codex/skills ~/.agents/skills; do
  test -f "$d/using-superpowers/SKILL.md" && echo "superpowers found: $d"
done
```

Any hit means superpowers is present — do not reinstall or update it; continue
with the flow.

## Installation

If no directory has it, install:

```bash
npx skills add https://github.com/obra/superpowers
```

The installer detects the current runtime and places the skills correctly. Node.js
must be available for `npx`; if it is not, tell your human partner superpowers
requires Node, and stop — do not proceed to implementation without it.

## If newly installed skills are not loadable this session

Some harnesses list skills only at session start. After installing, if the skill
list you can invoke does not yet include the superpowers skills, do not skip them
and do not wing their workflow from memory: open the installed files directly —
for example `~/.agents/skills/brainstorming/SKILL.md` — read each one, and follow
it exactly as written. The files are the methodology; following them from disk is
equivalent to invoking them.

## Naming varies by runtime

The same skill appears under different names depending on the harness: typically
`brainstorming`, sometimes prefixed like `superpowers:brainstorming`. Match on the
skill's base name, not the prefix.
