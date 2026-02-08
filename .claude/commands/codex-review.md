# Codex Review

Call Codex CLI to validate your assertions, findings, or plans by writing them to a file first.

## Instructions

1. Generate a unique filename and write your detailed findings:

```bash
FINDINGS_FILE=~/.claude/temp/codex-review-$(date +%s).md
```

Write to `$FINDINGS_FILE` including:
- Context (what you reviewed, branch name, etc.)
- Each finding with: location, evidence, reasoning, severity
- Questions you want validated
- Any assumptions you made

2. Call Codex to review the file:

```bash
codex exec "Review my findings in $FINDINGS_FILE. Validate each assertion - are they accurate? Any false positives? Did I miss anything? Read the file first."
```

3. After Codex responds:
   - Update your findings based on feedback
   - Remove false positives Codex identified
   - Add any issues Codex found that you missed
   - Present the revised assessment to the user

4. Clean up the temp file when done:

```bash
rm $FINDINGS_FILE
```
