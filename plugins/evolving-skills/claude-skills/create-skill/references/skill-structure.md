<overview>
Skills have three structural layers: YAML frontmatter for discovery, XML body content for execution, and supporting files for progressive disclosure.
</overview>

<frontmatter>
Required for all skills:
```yaml
---
name: skill-name
context_budget:
  skill_md: 150
  learnings_md: 40
---
```

Model-invoked skills also need a retrieval description:
```yaml
description: Create concise skill files. Use when authoring or improving SKILL.md files.
```

User-invoked-only skills should avoid accidental retrieval:
```yaml
disable-model-invocation: true
```
</frontmatter>

<name_rules>
`name:` must:
- Match the directory name
- Use lowercase letters, numbers, and hyphens
- Be 64 characters or fewer
- Avoid reserved names and vague helpers

Prefer verb-noun names:
- `create-skill`
- `audit-contracts`
- `manage-dns-records`
- `generate-concept-brief`

Avoid:
- `helper`
- `utils`
- `documents`
- Directory/name mismatches
</name_rules>

<description_rules>
For model-invoked skills, `description:` is functional code.

Rules:
- Third person
- Front-load the leading action word
- Say what the skill does and when to use it
- Avoid synonym stuffing
- Include handoff conditions only when another skill should invoke it

Good:
```yaml
description: Analyze Excel spreadsheets, create pivot tables, and generate charts. Use when working with spreadsheets, tabular data, or .xlsx files.
```

Weak:
```yaml
description: Helps with data.
```
</description_rules>

<xml_body>
Use pure XML tags in skill bodies. Do not use markdown headings for top-level structure.

Required tags:
- `<objective>`: what the skill accomplishes
- `<quick_start>`: fastest safe path for the common case
- `<success_criteria>` or `<when_successful>`: observable completion checks

Common optional tags:
- `<essential_principles>`
- `<intake>`
- `<routing>`
- `<references_index>`
- `<templates_index>`
- `<process>`
- `<validation>`
- `<examples>`
- `<anti_patterns>`

Keep markdown lists, tables, and code blocks inside XML tags when useful.
</xml_body>

<tag_rules>
Use semantic tag names:
- `<process>` for ordered execution
- `<success_criteria>` for done checks
- `<references_index>` for loadable files
- `<routing>` for branch selection

Avoid mixing tag names for the same concept. If one file uses `<process>`, do not use `<workflow>` elsewhere for the same role unless it has a distinct meaning.

Close every tag.
</tag_rules>

<directory_patterns>
Simple skill:
```text
skill-name/
├── SKILL.md
└── learnings.md
```

Router skill:
```text
skill-name/
├── SKILL.md
├── learnings.md
├── workflows/
├── references/
└── templates/
```

Reference skill:
```text
skill-name/
├── SKILL.md
├── learnings.md
└── resources/
```

Orchestrator skill:
```text
skill-name/
├── SKILL.md
├── learnings.md
├── workflows/
├── references/sub-agents.md
└── templates/
```
</directory_patterns>

<progressive_disclosure>
Progressive disclosure follows branch boundaries.

Keep inline:
- Rules every branch needs
- Routing and intake
- Short indexes with when-to-read pointers
- Success criteria

Move out:
- Branch-specific procedures
- Long examples
- Domain knowledge
- API details
- Reusable output templates

References should be one level deep from SKILL.md or the workflow that requires them.
</progressive_disclosure>

<reference_pointers>
Index entries should tell the agent when to read the file.

Good:
```xml
<references_index>
| Reference | Purpose |
|-----------|---------|
| references/api.md | Read when validating API endpoints or auth behavior |
</references_index>
```

Weak:
```xml
| references/api.md | API docs |
```
</reference_pointers>

<file_rules>
Use:
- Forward slashes in paths
- Descriptive filenames
- One concern per reference
- Root `learnings.md` for execution learnings
- `scripts/` for reusable I/O and validation

Avoid:
- Nested reference chains
- Files over the declared line budget
- Scripts that encode domain knowledge better stored in markdown
- Duplicate rules across SKILL.md and references
</file_rules>

<validation_checklist>
Before finalizing:
- [ ] Frontmatter mode is explicit
- [ ] `name` matches directory
- [ ] Model-invoked `description` has trigger language
- [ ] Body uses XML top-level tags
- [ ] Required tags are present
- [ ] References are indexed with when-to-read pointers
- [ ] Branch-specific content is outside SKILL.md
- [ ] Process steps have completion criteria
- [ ] Root `learnings.md` exists
- [ ] Line budgets are respected
</validation_checklist>
