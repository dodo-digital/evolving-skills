<overview>
Clear skills reduce interpretation work. Give the agent enough context, criteria, and examples to act predictably without restating knowledge it already has.
</overview>

<golden_rule>
Show the skill to someone with minimal context. If they cannot follow it, the agent will likely branch or guess incorrectly.
</golden_rule>

<context>
Include only context that changes execution:
- What the task result will be used for
- Who the output is for
- Which larger workflow the task belongs to
- What successful completion looks like

Avoid background that does not change a decision, tool call, validation step, or output shape.
</context>

<specificity>
Be specific where ambiguity would create different outcomes.

Weak:
```xml
<quick_start>
Process the data.
</quick_start>
```

Better:
```xml
<quick_start>
Extract customer names and email addresses from the CSV, remove duplicate emails, and save a JSON array to `output/customers.json`.
</quick_start>
```

Specificity should define inputs, transformations, outputs, edge cases, and validation when those details matter.
</specificity>

<sequential_steps>
Use ordered steps for workflows. Each step should have:
- Action
- Freedom level when helpful
- Completion criterion

Pattern:
```xml
<process>
1. **Extract records** [LOW freedom]
   Run `scripts/extract.py input.csv output/raw.json`.
   Done when `output/raw.json` exists and the command exits zero.

2. **Normalize records** [MEDIUM freedom]
   Deduplicate by email and standardize dates to YYYY-MM-DD.
   Done when every record has `name`, `email`, and `date` fields.
</process>
```
</sequential_steps>

<show_dont_tell>
When format matters, show an example. Examples communicate spacing, ordering, tone, and level of detail better than abstract rules.

Use examples for:
- Commit messages
- Reports
- JSON/YAML output
- Prompts
- File formats
- Voice and style

Keep examples complete enough to copy, but do not include many variants unless each variant changes behavior.
</show_dont_tell>

<avoid_ambiguity>
Avoid phrases that blur obligation.

| Ambiguous | Clear |
|-----------|-------|
| Try to validate | Always validate before proceeding |
| Should probably | Must, may, or skip when |
| Generally | Always except when |
| Consider | If X, then Y |

Use explicit exceptions:
```xml
<validation>
Always run `scripts/validate.py output/` after writing files. Skip only when the workflow creates no files.
</validation>
```
</avoid_ambiguity>

<edge_cases>
Define edge cases where the default answer would be risky or inconsistent.

Pattern:
```xml
<edge_cases>
- No records found: write `[]` and report zero matches
- Duplicate records: keep the newest by `updated_at`
- Invalid records: skip them and include count in validation notes
</edge_cases>
```

Do not enumerate obvious edge cases that standard agent judgment handles well.
</edge_cases>

<output_format>
When output format matters, specify it precisely.

Include:
- Required sections or fields
- Order
- Length limits
- File path
- Example output
- Validation command when available

Use exact templates for machine-read outputs. Use flexible defaults for human-facing prose where judgment should adapt to context.
</output_format>

<decision_criteria>
When the agent must choose, provide criteria instead of options.

Weak:
```xml
<process>
Choose the right chart.
</process>
```

Better:
```xml
<decision_criteria>
- Use bar charts for category comparisons with fewer than 10 categories
- Use line charts for trends over time
- Use scatter plots for relationships between two numeric variables
</decision_criteria>
```
</decision_criteria>

<requirements>
Separate priorities:
```xml
<requirements>
<must_have>
- Required inputs, actions, outputs, and validations
</must_have>

<nice_to_have>
- Enhancements to include when time and context allow
</nice_to_have>

<must_not>
- Prohibited tools, formats, sources, or side effects
</must_not>
</requirements>
```
</requirements>

<success_criteria>
Success criteria should be observable. Prefer checkboxes that can be verified by reading files, command output, or the final response.

Good:
- [ ] Output file exists at the required path
- [ ] Validation command exits zero
- [ ] Report contains all required sections
- [ ] User-facing answer states unresolved risks

Weak:
- [ ] Output is good
- [ ] The task is complete
</success_criteria>
