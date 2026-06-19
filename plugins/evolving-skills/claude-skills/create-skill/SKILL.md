---
name: create-skill
description: "Create Codex skills with context engineering built in. Use when building new skills, auditing existing skills, or adding components to skills. Supports four archetypes: simple, router, orchestrator, and reference."
context_budget:
  skill_md: 200
  max_references: 12
  learnings_md: 40
  sub_agent_limit: 500
---

<objective>
Create predictable, context-efficient Codex skills that make agent behavior repeatable without over-constraining judgment.
</objective>

<essential_principles>
These principles are non-negotiable for every skill created:

1. **Predictability First**
   - A skill exists to make the agent follow the same decision process each run
   - Predictable does not mean identical output; it means stable branching, loading, validation, and completion behavior
   - Prefer clear criteria over long explanations

2. **Progressive Disclosure**
   - Layer 1: Metadata (name, description) loads at startup (~50 tokens)
   - Layer 2: SKILL.md loads when triggered (max 200 lines for router skills)
   - Layer 3: References/workflows load on-demand during execution
   - Constraint: "How do I structure so Codex loads only what's needed?"

3. **Conciseness**
   - "The context window is a public good"
   - Only add context Codex doesn't already have
   - Delete no-op, duplicate, sediment, and sprawl content
   - Default assumption: Codex is already very smart

4. **Pure XML Structure**
   - Use XML blocks (`<objective>`, `<process>`) not markdown headings
   - 25% token savings vs markdown
   - Unambiguous section boundaries

5. **Required Reading Pattern**
   - Every workflow declares which references to load
   - Each pointer says when the file should load, not just what it contains
   - Context is branch-specific, not monolithic

6. **Skill Evolution**
   - Skills drift out of alignment over months — design for iterative change
   - Stable core (SKILL.md, workflows) + evolving periphery (`learnings.md`, knowledge files)
   - Every new skill includes a root `learnings.md`, a `learnings_md` budget, and a `<learning_capture>` block in SKILL.md
   - Scripts stay dumb I/O; domain knowledge lives in `.md` files Codex can update
   - See references/skill-evolution.md for patterns
</essential_principles>

<quick_start>
Most common use: Create a new skill

1. User describes what the skill should do
2. Classify archetype (simple/router/orchestrator/reference)
3. Generate SKILL.md with appropriate structure
4. If router+: generate workflows and references
5. Validate context budget

Run: "I need a skill that [description]"
</quick_start>

<intake>
What would you like to do?

1. **Create new skill** → workflows/create-simple.md or workflows/create-router.md
2. **Create orchestrator skill** (uses sub-agents) → workflows/create-orchestrator.md
3. **Create reference skill** (progressive disclosure) → workflows/create-reference.md
4. **Audit existing skill** → workflows/audit-context.md
5. **Verify skill is current** → workflows/verify-skill.md
6. **Add workflow to skill** → workflows/add-workflow.md
7. **Add reference to skill** → workflows/add-reference.md
8. **Upgrade simple to router** → workflows/upgrade-to-router.md
9. **Make skill adaptive/self-healing** → references/skill-evolution.md
10. **Add hooks to a skill** → references/skill-hooks.md
11. **Fix unreliable skill completion** → references/skill-hooks.md → invoke `create-hook`

Ask the user which they need, or infer from their request.
</intake>

<routing>
| User Intent | Archetype | Workflow |
|-------------|-----------|----------|
| Simple task, one workflow | Simple | workflows/create-simple.md |
| Multiple workflows, needs routing | Router | workflows/create-router.md |
| Needs sub-agents for context isolation | Orchestrator | workflows/create-orchestrator.md |
| Domain knowledge, progressive loading | Reference | workflows/create-reference.md |
| Check existing skill efficiency | - | workflows/audit-context.md |
| Verify skill content is still accurate | - | workflows/verify-skill.md |
| Add capability to existing | - | workflows/add-workflow.md |
| Add reference file | - | workflows/add-reference.md |
| Convert simple to complex | - | workflows/upgrade-to-router.md |
| Make skill adaptive/self-healing | - | references/skill-evolution.md |
| Add hooks to a skill | - | references/skill-hooks.md |
| Skill completion unreliable | - | references/skill-hooks.md → `create-hook` |
</routing>

<archetype_selection>
Help user select archetype:

**Simple** - SKILL.md (max 150 lines) + learnings.md
- Decision traces, goal tracking, basic utilities
- No sub-components needed
- One clear workflow

**Router** - SKILL.md + workflows/ + references/
- Multiple distinct use cases
- Shared references across workflows
- Examples: scrape-linkedin, daily-workflow

**Orchestrator** - Router + sub-agents
- Needs to gather context from multiple sources
- Context isolation critical (sub-agents return summaries)
- Examples: client-context, project-retrospective

**Reference** - Index + progressive-load resources
- Domain expertise documentation
- User needs different slices at different times
- Examples: attio-mcp-usage, gmail-mcp-usage
</archetype_selection>

<references_index>
| Reference | Purpose |
|-----------|---------|
| references/core-principles.md | Read when applying context engineering fundamentals |
| references/predictability-and-invocation.md | Read when choosing invocation mode, branches, descriptions, or pruning |
| references/context-budget.md | Read when setting or auditing token/line budgets |
| references/degrees-of-freedom.md | Read when calibrating specificity for workflow steps |
| references/sub-agent-patterns.md | Read when designing orchestrator or sub-agent behavior |
| references/xml-structure.md | Read when checking XML section structure |
| references/validation-checklist.md | Read when finalizing or auditing a skill |
| references/model-testing.md | Read when testing a skill across model strengths |
| references/scripts-pattern.md | Read when adding reusable scripts or validators |
| references/skill-evolution.md | Read when adding learnings or living knowledge files |
| references/learning-capture.md | Read when creating `learning_capture` and `learnings.md` |
| references/skill-hooks.md | Read when adding hooks or fixing unreliable completion |
| references/common-patterns.md | Read when adding templates, examples, anti-patterns, or validation loops |
| references/be-clear-and-direct.md | Read when tightening instructions, edge cases, or decision criteria |
| references/skill-structure.md | Read when designing directories, frontmatter, or file organization |
| references/workflows-and-validation.md | Read when designing complex workflows, checkpoints, or recovery |
</references_index>

<templates_index>
| Template | Use For |
|----------|---------|
| templates/simple-skill.md | Simple SKILL.md + learnings.md skills |
| templates/router-skill.md | Multi-workflow skills |
| templates/orchestrator-skill.md | Sub-agent skills |
| templates/reference-skill.md | Progressive disclosure skills |
| templates/workflow-file.md | Individual workflow files |
| templates/sub-agent-definition.md | Sub-agent prompt structure |
</templates_index>

<learning_capture>
A learning is an actionable discovery from executing this skill that should change future runs. Save one dated, atomic entry to `learnings.md` when execution hits friction: repeated failed searches, changed data/tool structure, unexpected behavior, resolved errors, or a user correction/preference signal. Include the trigger, what failed or changed, the successful resolution, and the future rule. Do not save project facts, transient task details, or outputs that belong in deliverables.
</learning_capture>

<frontmatter_validation>
Every skill MUST have valid YAML frontmatter. Validate before completion:

**Required fields:**
- `name:` - Skill identifier (kebab-case)
- `description:` - What it does AND when to use, unless `disable-model-invocation: true`
- `context_budget:` - Token budget declaration
- `learnings_md:` - Learning file budget under `context_budget`

**Validation rules:**
1. Frontmatter block exists (starts with `---`)
2. Model-invoked skills have a front-loaded `description:` with trigger phrases ("Use when...", "Invoke for...")
3. `context_budget:` declares at minimum `skill_md:` limit
4. `context_budget:` declares `learnings_md:` limit
5. User-invoked-only skills set `disable-model-invocation: true`
6. Description is functional code - it's how semantic matching finds skills
</frontmatter_validation>

<success_criteria>
- [ ] Skill follows selected archetype structure
- [ ] SKILL.md under line limit for archetype
- [ ] Frontmatter has name, description, context_budget
- [ ] Invocation mode is explicit and description fits that mode
- [ ] context_budget declared in frontmatter
- [ ] Root `learnings.md` created with parseable entry format
- [ ] SKILL.md includes `<learning_capture>` defining what to save and what not to save
- [ ] All workflows have `<required_reading>`
- [ ] Each process step has a completion criterion
- [ ] Pure XML structure (no markdown headings in body)
- [ ] success_criteria uses checkbox format
</success_criteria>
