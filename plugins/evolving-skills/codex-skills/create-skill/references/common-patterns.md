<overview>
Common skill-authoring patterns. Use these when the skill needs examples, defaults, validation loops, or anti-patterns beyond the core structure.
</overview>

<template_pattern>
Use templates when output shape matters.

Strict templates are for compliance, automation, configs, and machine-read formats:
```xml
<report_structure>
ALWAYS use this exact markdown structure:

```markdown
# [Title]

## Summary
[One paragraph]

## Findings
- [Finding with evidence]

## Recommendations
1. [Action]
```
</report_structure>
```

Flexible templates are for exploratory or creative tasks:
```xml
<report_structure>
Use this as the default structure, but adapt sections to the evidence:
...
</report_structure>
```

Choose one default. Add an escape hatch only when real edge cases need it.
</template_pattern>

<examples_pattern>
Use input/output examples when rules are too abstract.

Good examples show:
- Exact formatting
- Tone and length
- Edge cases
- What to omit

Pattern:
```xml
<examples>
<example number="1">
<input>{representative input}</input>
<output>{complete expected output}</output>
</example>
</examples>
```

Do not add examples that merely restate the same pattern.
</examples_pattern>

<terminology>
Pick one term and use it throughout the skill.

Examples:
- Use "workflow" or "process", not both for the same concept
- Use "reference", not a mix of "guide", "doc", "knowledge file", and "resource" unless those are distinct directories
- Use "extract", not a mix of "pull", "get", and "retrieve"

Inconsistent terms create hidden branches.
</terminology>

<default_with_escape_hatch>
Provide one default approach with one escape hatch.

Good:
```xml
<quick_start>
Use pdfplumber for text extraction. For scanned PDFs requiring OCR, use pdf2image with pytesseract instead.
</quick_start>
```

Weak:
```xml
<quick_start>
Choose from pypdf, pdfplumber, PyMuPDF, pdfminer, tabula, or OCR depending on your needs.
</quick_start>
```
</default_with_escape_hatch>

<validation_pattern>
For fragile or file-producing workflows, validate immediately after the relevant step.

Pattern:
```xml
<validation>
Run:
```bash
python scripts/validate.py output/
```

If validation fails, fix the error and rerun. Proceed only when validation exits zero.
</validation>
```

Validation errors should be specific:
- Include location
- Include expected vs actual value
- Include valid options when possible
- Suggest a fix when deterministic
</validation_pattern>

<checklist_pattern>
Use a checklist when:
- Workflow has 5+ ordered steps
- Steps are easy to skip
- The task may be interrupted and resumed
- Progress tracking materially reduces mistakes

Pattern:
```xml
<workflow>
Copy this checklist and update it as you work:
- [ ] Step 1: Analyze input
- [ ] Step 2: Create plan
- [ ] Step 3: Validate plan
- [ ] Step 4: Execute
- [ ] Step 5: Verify output
</workflow>
```
</checklist_pattern>

<progressive_disclosure_pattern>
Keep SKILL.md focused on always-needed rules and routing. Move branch-specific detail to workflows or references.

Good:
```xml
<references_index>
| Reference | Purpose |
|-----------|---------|
| references/forms.md | Read when filling or editing PDF form fields |
| references/redlining.md | Read when tracked changes are required |
</references_index>
```

Weak:
```xml
<references_index>
| references/forms.md | Forms |
| references/redlining.md | Redlining |
</references_index>
```
</progressive_disclosure_pattern>

<anti_patterns>
Avoid these patterns:

| Anti-pattern | Fix |
|--------------|-----|
| Vague description | Write what it does and when it triggers |
| First-person description | Use third person |
| Markdown headings in skill body | Use XML tags |
| Unclosed XML tags | Close every tag |
| Too many options | Pick a default plus one escape hatch |
| Nested references | Link directly from SKILL.md or workflow |
| Weak pointers | Say when to read the file |
| No completion criteria | Add "Done when..." to each step |
| Duplicated rules | Keep one canonical source |
| Stale sediment | Delete rules from older skill versions |
</anti_patterns>

<dynamic_context_warning>
When documenting dynamic context syntax or file-reference syntax, prevent accidental execution in examples.

Use spaces in documentation examples:
- `! \`git status\`` instead of executable dynamic context syntax
- `@ package.json` instead of direct file-reference syntax

Tell the user to remove the space only in actual use.
</dynamic_context_warning>
