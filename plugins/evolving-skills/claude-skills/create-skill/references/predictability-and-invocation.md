<overview>
Skill quality is measured by predictability: after the skill loads, the agent should choose the same branch, load the same relevant context, validate the same criteria, and know when the task is complete.
</overview>

<predictability>
Predictability does not mean deterministic output. It means repeatable process.

Use skills to pin down:
- When the skill should activate
- Which branch applies
- Which context files load for that branch
- What must happen before moving to the next step
- What proves the task is complete

Do not use skills to restate general model knowledge, preserve old wording, or force creative output into a brittle script.
</predictability>

<invocation_modes>
Choose the invocation mode before writing the body.

| Mode | Frontmatter | Use When |
|------|-------------|----------|
| Model-invoked | `description:` with trigger phrases | The model should discover and load the skill from ordinary user requests |
| User-invoked only | `disable-model-invocation: true` | The user will explicitly invoke the skill and model discovery would create false positives |
| Router | Broad `description:` plus routing table | One semantic trigger leads to several mutually exclusive branches |

For model-invoked skills, the description is functional code. It is the first branch selector.

For user-invoked-only skills, do not over-engineer trigger phrases. Focus the body on execution after invocation.
</invocation_modes>

<description_rules>
Write descriptions for retrieval and routing, not prose polish.

Rules:
- Start with the leading word that should match the user's intent, such as "Create", "Audit", "Transcribe", "Schedule", or "Analyze"
- Include what the skill does and when to use it
- Name the trigger condition once; avoid synonym stuffing
- Do not include identity already obvious from `name:`
- Add "Use when..." or equivalent only for model-invoked skills
- If another skill should reach this one, include that handoff condition explicitly

Good:
```yaml
description: Audit existing Codex skills for predictability, context efficiency, routing, and validation gaps. Use when reviewing or improving SKILL.md files.
```

Weak:
```yaml
description: Helps with skills, skill writing, authoring, prompts, agents, routing, and docs.
```
</description_rules>

<branch_design>
Use branches as the main progressive-disclosure test.

Keep content inline only when every branch needs it. Move branch-specific content to a workflow or reference.

For each branch, define:
- Trigger: when this branch applies
- Required reading: files needed only for this branch
- Steps: ordered actions for this branch
- Completion criterion: how the agent knows the branch is done

If two branches share fewer than half their steps, split them. If two branches differ only by a small condition, keep one workflow and add a decision rule.
</branch_design>

<context_pointers>
Pointers must say when to read, not just what exists.

Weak pointer:
```xml
| references/api.md | API docs |
```

Better pointer:
```xml
| references/api.md | Read when the workflow calls external API endpoints or validates API claims |
```

In `<required_reading>`, include only files needed to execute the selected branch. If a file is useful background but not necessary, leave it out.
</context_pointers>

<step_completion>
Every process step should have a completion criterion.

Pattern:
```xml
<process>
1. **Classify invocation** [MEDIUM freedom]
   Decide whether the skill is model-invoked, user-invoked-only, or a router.
   Done when the frontmatter mode is explicit and consistent with the routing surface.
</process>
```

Completion criteria prevent skipped steps and make validation cheaper than rereading the whole task.
</step_completion>

<pruning_rules>
Prune at sentence level.

Delete or rewrite content that fails any test:
- **No-op**: removing it does not change future agent behavior
- **Duplicate**: the same rule already appears in a canonical place
- **Sediment**: old guidance remains after the skill changed direction
- **Sprawl**: a file mixes unrelated branches or concerns
- **Weak pointer**: a referenced file exists, but the skill does not say when to load it

Prefer one strong rule in the canonical file over several weaker repeats.
</pruning_rules>

<audit_additions>
When auditing a skill, add these checks:
- [ ] Invocation mode is explicit
- [ ] Model-invoked descriptions have front-loaded trigger words
- [ ] Router branches are mutually exclusive or have clear priority
- [ ] Required-reading pointers say when to read each file
- [ ] Each process step has a completion criterion
- [ ] No-op, duplicate, sediment, and sprawl content has been removed
</audit_additions>
