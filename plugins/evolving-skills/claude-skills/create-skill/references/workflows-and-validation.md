<overview>
Workflow design patterns for skills that execute multi-step, fragile, or file-producing work.
</overview>

<workflow_steps>
Every workflow step should include:
- Action
- Freedom level when useful
- Inputs or tools
- Completion criterion
- Recovery path for expected failures

Pattern:
```xml
<process>
1. **Create plan** [MEDIUM freedom]
   Write `changes.json` with the intended edits.
   Done when every requested change has a structured entry.

2. **Validate plan** [LOW freedom]
   Run `python scripts/validate_changes.py changes.json`.
   Done when validation exits zero.
</process>
```
</workflow_steps>

<checklist_pattern>
Use a checklist when ordering matters or the workflow may be interrupted.

```xml
<workflow>
Track progress:
- [ ] Step 1: Analyze input
- [ ] Step 2: Build plan
- [ ] Step 3: Validate plan
- [ ] Step 4: Execute plan
- [ ] Step 5: Verify output
</workflow>
```

Checklists are not necessary for short, low-risk workflows.
</checklist_pattern>

<validate_fix_repeat>
Use this loop for artifacts that can be checked mechanically:

1. Produce or edit artifact
2. Run validator
3. If validator fails, fix the specific error
4. Rerun validator
5. Proceed only after zero errors

Validation commands belong in the workflow. Reusable validator code belongs in `scripts/`.
</validate_fix_repeat>

<plan_validate_execute>
Use plan-validate-execute when mistakes are expensive or hard to undo.

```xml
<process>
1. **Analyze requirements** [MEDIUM freedom]
   Identify all intended changes.
   Done when no requirement lacks an implementation target.

2. **Create plan** [LOW freedom]
   Write `changes.json`.
   Done when the plan is machine-readable.

3. **Validate plan** [LOW freedom]
   Run `python scripts/validate_changes.py changes.json`.
   Done when validation exits zero.

4. **Execute plan** [LOW freedom]
   Run `python scripts/apply_changes.py changes.json`.
   Done when the script reports all changes applied.

5. **Verify output** [MEDIUM freedom]
   Inspect changed artifacts and rerun validation if available.
   Done when success criteria pass.
</process>
```
</plan_validate_execute>

<conditional_workflows>
Use conditional workflows when different task types require different approaches.

```xml
<decision_point>
Choose one branch:
- Creating new content → workflows/create.md
- Editing existing content → workflows/edit.md
- Auditing content → workflows/audit.md
</decision_point>
```

Branches should be mutually exclusive. If they overlap, document priority order.
</conditional_workflows>

<validation_scripts>
Good validation scripts:
- Validate inputs before writing output
- Exit nonzero on failure
- Print exact file, line, field, or key when possible
- Include expected vs actual values
- List valid options for enum-like fields
- Avoid business decisions that belong in markdown references

Good error:
```text
Field 'status' has invalid value 'pending_review'. Valid values: active, paused, archived.
```

Weak error:
```text
Invalid field.
```
</validation_scripts>

<checkpoints>
Add checkpoints when a workflow has phases that can fail independently.

Pattern:
```xml
<phase_1>
Collect data.
CHECKPOINT: Continue only when required fields are present.
</phase_1>

<phase_2>
Transform data.
CHECKPOINT: Continue only when validation passes.
</phase_2>
```

Checkpoints prevent early errors from contaminating later steps.
</checkpoints>

<error_recovery>
Define recovery for expected failures.

Pattern:
```xml
<error_recovery>
If validation fails:
- Read the error
- Fix the named file/field
- Rerun validation
- If the same failure repeats three times, stop and report diagnostics
</error_recovery>
```

Escalate when the input is corrupted, the external system changed shape, or the task requires a decision the user has not delegated.
</error_recovery>

<success_criteria>
A workflow is well-designed when:
- [ ] Branch choice is deterministic or priority is documented
- [ ] Steps have completion criteria
- [ ] Fragile operations have validation
- [ ] Validation failures have recovery paths
- [ ] Scripts are reusable and markdown keeps domain knowledge
- [ ] Final success criteria are observable
</success_criteria>
