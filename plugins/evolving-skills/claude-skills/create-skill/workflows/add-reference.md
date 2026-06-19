<required_reading>
- references/core-principles.md
- references/predictability-and-invocation.md
</required_reading>

<objective>
Add a new reference file to an existing skill.
</objective>

<process>
1. **Read existing skill**
   - Read SKILL.md
   - List existing references
   - Understand the domain

2. **Gather requirements**
   - What knowledge does this reference contain?
   - Which workflows will use it?
   - Is this truly new, or should it extend existing reference?
   - What exact condition should make an agent read it?
   Done when the reference has one concern and one loading condition.

3. **Write the reference**
   Create `references/{new-reference}.md`:
   - Keep under 300 lines
   - Self-contained (no dependencies on other refs)
   - Include practical examples
   - Focus on what Claude doesn't already know
   Done when it passes the no-op, duplicate, sediment, and sprawl checks.

4. **Update SKILL.md**
   Add to references_index:
   ```
   | references/new-reference.md | Read when {loading condition} |
   ```

5. **Update workflows that need it**
   Add to `<required_reading>` in relevant workflows

6. **Validate**
   - [ ] Under 300 lines
   - [ ] Self-contained
   - [ ] Pointer says when to read it
   - [ ] Indexed in SKILL.md
   - [ ] Added to relevant workflow required_reading
</process>

<success_criteria>
- [ ] Reference file created
- [ ] SKILL.md references_index updated
- [ ] Relevant workflows updated
- [ ] Reference is self-contained
</success_criteria>
