# Skill Hook Caveats For Codex

Codex skills do not support Claude-style skill-scoped hooks in YAML frontmatter. Do not add `hooks:` blocks to Codex skills and do not rely on Stop, PreToolUse, PostToolUse, SessionStart, or SubagentStop events firing from a skill.

## What To Use Instead

For reliability in Codex skills:

1. Put explicit validation steps in `<process>`.
2. Put objective completion checks in `<success_criteria>`.
3. Use scripts in `scripts/` for deterministic validation when the checks are fragile.
4. Tell the agent when to run the validation script and how to interpret failures.
5. Keep reusable knowledge in markdown references, not in scripts.

## Validation Pattern

```xml
<process>
1. Make the requested changes.
2. Run `scripts/validate.py --target {path}`.
3. If validation fails, fix the reported issue and rerun once.
4. Report remaining failures to the user with the exact command output.
</process>

<success_criteria>
- [ ] Validation command passed
- [ ] User-facing output includes any residual risks
</success_criteria>
```

## When The User Needs Hooks

If the user is explicitly creating a Claude Code skill or maintaining a `.claude/skills` project, Claude hook documentation may still be relevant to that target ecosystem. Keep that content in a target-specific reference rather than adding hook instructions to a Codex skill by default.

## Script Organization

```
my-skill/
├── SKILL.md
├── scripts/
│   ├── validate.py
│   └── check-output.sh
├── workflows/
└── references/
```

Workflow says when to validate. Scripts enforce how. Keep scripts as pure validation with dumb I/O; domain logic belongs in markdown the agent can read and update.
